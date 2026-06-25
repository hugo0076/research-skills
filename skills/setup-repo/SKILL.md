---
name: setup-repo
description: Scaffold a new research project repository, then run the intake briefing to populate its direction. Use when Hugo starts a new research direction or asks to set up a project repo. Picks the host (Spartan or local), assembles and fills the orchestrator CLAUDE.md, seeds the docs files, sets up local git and pre-commit, then invokes request-briefing. Does not create a GitHub remote (Hugo wires that himself).
---

# setup-repo

Two phases: scaffold the skeleton, then run the briefing to fill it. Scaffold first, so the briefing has files to write into. Run from where the project should live.

## Phase 0: name and host
- **Project name** (ask Hugo, or take it from the directory name). It names the directory and fills the `[PROJECT]` placeholder.
- **Host**, because the orchestrator brief is host-specific. If the project lives under `/data/gpfs/projects/punim2265` (Spartan) the host is **spartan**; on a local workstation (hyperpc) it is **local**. Ask if unsure. The host decides which "Where you run" / compute block goes into CLAUDE.md and whether `sbatch`/`slurm-dispatch` apply at all.

## Phase 1: scaffold
### Structure to create
- `docs/project_direction.md` — the standing research brief. **Canonical source of the direction.** Seed with a one-line placeholder; the briefing fills it.
- `docs/experiment_log.md` — dated evidence log (the product). Seed with a top line `Direction: ./project_direction.md` and an otherwise empty history.
- `docs/claims_status.md` — the claims ledger (see the `experiment-log` skill). Seed with its header.
- `docs/failure_log.md` — what failed and why. Seed with a header.
- `data/` — datasets (git-ignored).
- `papers/` — reference PDFs the orchestrator can read.
- `outputs/` — plots, results, job/run outputs (git-ignored).
- `src/` — source.
- `CLAUDE.md` — assemble from templates (next step), then fill.

Do **not** create `logs/` or `tests/` by default. Add `tests/` only if the project actually needs it. The `.claude/` folder is created by Claude Code automatically; do not create it.

### Steps
1. Create the directories and the four `docs/` files with their seed headers (see `experiment-log` for what goes in the log and ledger).
2. **Assemble CLAUDE.md** by concatenating the head, the chosen host block, and the shared role:
   `cat ~/research-skills/templates/head.md ~/research-skills/templates/host-<host>.md ~/research-skills/templates/role-core.md > CLAUDE.md`
   (where `<host>` is `spartan` or `local`). Then replace `[PROJECT]` with the project name and fill the PROJECT-SPECIFIC block (stack, commands, layout) as far as you can now. The rest firms up after the briefing.
3. Run `uv init`. Set `requires-python` in `pyproject.toml` to a modern version (e.g. `">=3.11"`); otherwise uv falls back to an old system Python. uv will fetch the pinned version.
4. Set `UV_CACHE_DIR` appropriately. On Spartan, point it at project storage so the `/home` quota is not hit, e.g. `export UV_CACHE_DIR=/data/gpfs/projects/punim2265/.uv-cache`. Locally the default cache is fine.
5. Ensure `.gitignore` ignores `data/`, `outputs/`, and `.venv/`.
6. Write `.pre-commit-config.yaml` (below) and run `pre-commit install`.
7. Make the initial commit on the **local** git repo only. Do **not** create a GitHub remote: Hugo wires it up himself (private repo, lowercase-dashed name).

## Phase 2: briefing (deferrable)
Once the skeleton exists, invoke `request-briefing` to interview Hugo and populate `docs/project_direction.md` (and seed `docs/claims_status.md`). Do this before designing or launching any experiments. If Hugo wants to scaffold only for now, stop after Phase 1 and leave `docs/project_direction.md` as a placeholder for a later briefing. Commit again once the direction is written.

## .pre-commit-config.yaml
```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.9.1
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v5.0.0
    hooks:
      - id: check-yaml
      - id: check-toml
      - id: end-of-file-fixer
      - id: trailing-whitespace
      - id: check-added-large-files
        args: ['--maxkb=1000']
      - id: check-merge-conflict
      - id: detect-private-key
```
