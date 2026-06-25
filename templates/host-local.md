## Where you run
You run on **hyperpc**, Hugo's local workstation, with the project repo on a local SSD. A single **RTX 4080 (16GB VRAM)** is directly available to you. No scheduler, no queue. Internet works as normal.

## Compute
One GPU with 16GB, so the constraints invert a cluster's:
- **Run experiments as background processes** so you stay responsive, e.g. `nohup uv run python <script> > outputs/<run>/run.log 2>&1 &` (or a dedicated tmux pane). Poll the logfile and check the process / exit code for completion; read results from `outputs/<run>/`.
- **Serial by default.** There is one GPU, so GPU jobs run one at a time. Only overlap two if they genuinely co-fit in 16GB (rare for training). CPU-bound prep (data, tokenization, analysis) can run alongside a GPU job. This is the opposite of the cluster's concurrency default.
- **Mind the 16GB ceiling.** Prefer smaller models, gradient checkpointing, mixed precision, LoRA / quantization, modest batch sizes. If something genuinely will not fit, that is a signal it belongs on Spartan's H100s, flag it to Hugo rather than thrashing.
- Quick smoke checks (a few minutes, small) can run inline; anything longer goes to the background.
- There is no `slurm-dispatch` here and no smoke-bench to hold; the GPU is just there to use.

