---
name: setup-repo
description: Scaffold a new research project repository. Use when Hugo starts a new research direction or asks to set up a project repo. Creates the standard directory structure, runs uv init, sets up local git and pre-commit, and seeds the docs files. Does not create a GitHub remote (Hugo wires that himself).
---

# setup-repo

Run from where the project should live. On Spartan that is under `/data/gpfs/projects/punim2265/<project>`.

## Structure to create
- `docs/experiment_log.md` — the durable evidence log (see the `experiment-log` skill).
- `docs/claims_status.md` — the claims ledger (see the `experiment-log` skill).
- `data/` — datasets (git-ignored).
- `papers/` — reference PDFs the orchestrator can read.
- `outputs/` — plots, results, job outputs (git-ignored).
- `src/` — source.
- `CLAUDE.md` — copy the orchestrator brief template in.

Do **not** create `logs/` or `tests/` by default. Add `tests/` only if the project actually needs it.

## Steps
1. Create the directories and the two `docs/` files, seeded with minimal headers (see `experiment-log` for what goes in them).
2. Run `uv init` (creates `pyproject.toml`, a local git repo, a `.gitignore`, and a README). Set `requires-python` in `pyproject.toml` to a modern version (e.g. `">=3.11"`); otherwise uv falls back to the system Python (3.9 on Spartan). uv will fetch the pinned version.
3. On Spartan, set `UV_CACHE_DIR` onto project storage so the small `/home` quota is not hit, e.g. `export UV_CACHE_DIR=/data/gpfs/projects/punim2265/.uv-cache`. The `.venv` lives in the project dir on GPFS, which is fine.
4. Ensure `.gitignore` ignores `data/`, `outputs/`, and `.venv/`. (uv usually ignores `.venv` and its caches; add `data/` and `outputs/`. Caches are auto-ignored.)
5. Write `.pre-commit-config.yaml` (below) and run `pre-commit install`.
6. Make the initial commit on the **local** git repo only. Do **not** create a GitHub remote: Hugo wires it up himself (private repo, lowercase-dashed name).

The `.claude/` folder is created by Claude Code automatically; do not create it.

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
