# Global preferences
- Use uv, never pip or conda. Use ruff, never black or flake8.
- Type hints on all function signatures. pathlib.Path, never os.path.
- Conventional commits: feat:, fix:, experiment:, docs:, refactor:
- Use Read/Glob/Grep tools instead of cat/find/grep via Bash.
- Don't declare victory without running the code. If you fail twice on the same fix, stop and ask.
- When uncertain about a scientific claim, say so explicitly.
- marimo notebooks are .py files: `@app.cell` decorators, unique descriptive variable names across cells (e.g. `fig_roc_benign` not `fig`), no Jupyter. Can't access a UI element's `.value` in the same cell it's defined.
- Never mention Claude, AI, or LLMs in commit messages, code comments, or added to files. Write commits as if a human wrote them.

# HPC (Spartan, University of Melbourne)
- SSH host: `hlyonskeenan@spartan.hpc.unimelb.edu.au`
- Project dir: `/data/gpfs/projects/punim2265/` (shared project storage)
- Rsync template: `rsync -avh --progress hlyonskeenan@spartan.hpc.unimelb.edu.au:/data/gpfs/projects/punim2265/metacognitive_monitoring/<PATH>/ <LOCAL_PATH>/`
