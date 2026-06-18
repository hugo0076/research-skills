---
name: setup-repo
description: Scaffold a new research project repository, then run the intake briefing to populate its direction. Use when Hugo starts a new research direction or asks to set up a project repo. Creates the standard structure, copies and fills the orchestrator CLAUDE.md, seeds the docs files, sets up local git and pre-commit, then invokes request-briefing. Does not create a GitHub remote (Hugo wires that himself).
---

# setup-repo

Two phases: scaffold the skeleton, then run the briefing to fill it. Scaffold first, so the briefing has files to write into. Run from where the project should live; on Spartan that is under `/data/gpfs/projects/punim2265/<project>`.

## Phase 0: project name
Get the project name up front (ask Hugo, or take it from the directory name). It names the directory and fills the `[PROJECT]` placeholder in CLAUDE.md. Everything else can wait for the briefing.

## Phase 1: scaffold
### Structure to create
- `docs/project_direction.md` — the standing research brief. **Canonical source of the direction.** Seed with a one-line placeholder; the briefing fills it.
- `docs/experiment_log.md` — dated evidence log (the product). Seed with a top line `Direction: ./project_direction.md` and an otherwise empty history.
- `docs/claims_status.md` — the claims ledger (see the `experiment-log` skill). Seed with its header.
- `docs/failure_log.md` — what failed and why. Seed with a header.
- `data/` — datasets (git-ignored).
- `papers/` — reference PDFs the orchestrator can read.
- `outputs/` — plots, results, job outputs (git-ignored).
- `src/` — source.
- `CLAUDE.md` — copy the orchestrator brief template in; replace `[PROJECT]` with the project name and fill the PROJECT-SPECIFIC block (stack, commands, layout) as far as you can now. The rest firms up after the briefing.

Do **not** create `logs/` or `tests/` by default. Add `tests/` only if the project actually needs it. The `.claude/` folder is created by Claude Code automatically; do not create it.

### Steps
1. Create the directories and the four `docs/` files with their seed headers (see `experiment-log` for what goes in the log and ledger).
2. Copy the orchestrator `CLAUDE.md` template in and fill `[PROJECT]` plus the PROJECT-SPECIFIC block.
3. Run `uv init`. Set `requires-python` in `pyproject.toml` to a modern version (e.g. `">=3.11"`); otherwise uv falls back to the system Python (3.9 on Spartan). uv will fetch the pinned version.
4. On Spartan, set `UV_CACHE_DIR` onto project storage so the small `/home` quota is not hit, e.g. `export UV_CACHE_DIR=/data/gpfs/projects/punim2265/.uv-cache`. The `.venv` lives in the project dir on GPFS, which is fine.
5. Ensure `.gitignore` ignores `data/`, `outputs/`, and `.venv/`.
6. Write `.pre-commit-config.yaml` (below) and run `pre-commit install`.
7. Make the initial commit on the **local** git repo only. Do **not** create a GitHub remote: Hugo wires it up himself (private repo, lowercase-dashed name).

## Phase 2: briefing
Once the skeleton exists, invoke `request-briefing` to interview Hugo and populate `docs/project_direction.md` (and seed `docs/claims_status.md`). Do this before designing or launching any experiments. Commit again once the direction is written.

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
