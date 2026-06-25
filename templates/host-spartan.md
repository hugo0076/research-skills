## Where you run
You run on a Spartan **sapphire** CPU node, with the project repo on GPFS local to you. No GPU is visible: if something needs a GPU, that is a job you submit (or the smoke-bench below), never something you run inline. Internet works from here as normal.

## What you're juggling
At any time you are doing several things at once, and not all of them are tight loops: running experiments to test hypotheses, keeping a GPU smoke-bench available, and continuously thinking about where the project is and what should come next. Do not force everything into "experiments"; the background thinking and the periodic check-ins with Hugo matter as much as the runs.

Keep a rolling **H100 smoke-bench**: hold a 24h GPU allocation and request a fresh one once the current drops below ~6h left (the 18h mark), so there is no gap. The interactive form is `sinteractive -p gpu-h100 --gres=gpu:1 --time 24:00:00 --cpus-per-task 16 --mem 200G`; for autonomous, non-interactive use hold it via `salloc` + `ssh` to the allocated node (see `slurm-dispatch`).

Compute routing:
- Under ~1 hour and fits in one H100's 80GB → run it on the smoke-bench. These can overlap while they fit in memory.
- Longer, or needs the full 80GB for over an hour → a batch job via `sbatch`.
- Compute is the bottleneck and the scheduler absorbs queue depth, so default toward running independent variants concurrently. If you are sweeping a hyperparameter or trying a few seeds or ablations you would likely want anyway (say a new loss-term weight at 0.01, 0.1, and 1.0), enqueue them all at once and collect the results together, rather than running one and waiting to decide. Reserve serial execution for when a run is expensive and its outcome would clearly determine whether the others are worth doing at all. When unsure, lean concurrent, and check with Hugo if the call is significant.

