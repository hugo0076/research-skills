---
name: slurm-dispatch
description: How to run compute on Spartan. Submit batch jobs, hold and use the interactive GPU bench, detect job completion, retrieve results, and keep your own allocation alive. Load for anything that touches compute.
---

# slurm-dispatch

## Environment basics
- You can submit jobs from anywhere (login node and compute node both work). Submit from the project dir on GPFS for IO. Account is `punim2265`. Internet is direct, no proxy.
- `uv` lives in `~/.local/bin`, which is shared across all nodes. **Job scripts run non-interactively and do not source `~/.bashrc`**, so every job script must put it on PATH (`export PATH="$HOME/.local/bin:$PATH"`) before any `uv` call, or `uv` will be "command not found".
- Pin a modern Python per project (`uv` will otherwise fall back to the system 3.9). Set `requires-python` in `pyproject.toml` and let `uv` fetch it.

## Batch jobs (the real runs)
```bash
#!/bin/bash
#SBATCH --account=punim2265
#SBATCH -p gpu-h100
#SBATCH --gres=gpu:1
#SBATCH --time=<HH:MM:SS>
#SBATCH --cpus-per-task=8
#SBATCH --mem=64G
#SBATCH -o outputs/<job>/slurm-%j.out
export PATH="$HOME/.local/bin:$PATH"
export UV_CACHE_DIR=/data/gpfs/projects/punim2265/.uv-cache
cd /data/gpfs/projects/punim2265/<project>
uv run python <script.py> <args>
echo done > outputs/<job>/DONE   # sentinel on success
```
- Defaults: `gpu-h100`, 1 GPU for an 8B SFT (A100 has similar queue waits, so just ask for H100). Adjust `--mem` on the fly; lower `--cpus-per-task` if 8 slows scheduling. For multi-GPU, consult the Spartan GPU docs and set `--gres=gpu:N` with matching cpus/mem.
- **Default to concurrency.** For an independent sweep (e.g. a loss-weight at 0.01 / 0.1 / 1.0), submit all jobs at once and let the scheduler queue them. Go serial only when a run is expensive and its outcome would decide whether the others are worth doing.

## Completion detection (validated)
```bash
jid=$(sbatch --parsable job.slurm)
while squeue -j "$jid" -h | grep -q .; do sleep <interval>; done   # job left the queue
sacct -j "$jid" -X --format=State,ExitCode,Elapsed
```
- Success = `State=COMPLETED` and `ExitCode=0:0`. Anything else (`FAILED`, `TIMEOUT`, `OUT_OF_MEMORY`, `CANCELLED`) means read the `.out` and the State and decide. `-X` gives the single allocation row, not the `.batch`/`.extern` steps.
- Read results from `outputs/<job>/` and confirm the `DONE` sentinel.

## Interactive GPU bench (quick tests)
Hold a bench with a sleeper job, then run smoke tests on the allocated node:
```bash
#!/bin/bash
#SBATCH --account=punim2265
#SBATCH -p gpu-h100
#SBATCH --gres=gpu:1
#SBATCH --time=24:00:00
#SBATCH --cpus-per-task=8 --mem=64G
#SBATCH -o outputs/bench/slurm-%j.out
sleep 86400
```
- Get the node: `node=$(squeue -j "$jid" -h -o %N)`.
- Run on it (primary, no ssh): `srun --jobid "$jid" --overlap --gres=gpu:1 bash -lc 'cd <project> && export PATH=$HOME/.local/bin:$PATH && uv run python smoke.py'`. Fallback: `ssh "$node" '...'`. Both are confirmed working.
- **Routing:** anything ≤ ~1h that fits in 80GB runs on the bench (these can overlap while they fit in memory); anything longer, or that needs the full 80GB for over an hour, becomes a batch job.
- **Renewal:** when the bench passes the 18h mark, decide whether you will actually need it soon. If you have nothing blocked on a quick test (e.g. only multi-day runs are in flight), do not hold one, flag to grab a bench when you next need it. The goal is to never burn a GPU slot idling, and never have to wait hours for a bench when you do need one.

## Keeping yourself alive (the parked orchestrator)
You run inside a 7-day `sinteractive` on the `sapphire` partition, in a tmux session on the login node, with a small supervisor loop wrapping `claude --resume <id> --rc`. If you exit, the loop relaunches you and the allocation is retained. Hugo renews the weekly allocation manually for now.

## Driving the rhythm (optional: /goal + /loop)
- After the briefing, set the direction's success criteria as a `/goal`.
- Run the monitoring rhythm as a `/loop` (~every 20–30 min): check `squeue`, pull finished results (`sacct` + read `outputs/`), update the experiment log, run break-tests on any positives, and ping Hugo at forks. This replaces a hand-rolled poll loop. (`/loop` is session-scoped, which is fine since the parked session is persistent.)
