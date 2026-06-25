<!-- Orchestrator brief. Loads every session (cwd + ancestors + ~/.claude); keep it lean. Fill the PROJECT-SPECIFIC block below on setup. The research direction/science goes in docs/project_direction.md, NOT here. -->

# CLAUDE.md — Research Orchestrator for [PROJECT]

<!-- ================ PROJECT-SPECIFIC: fill on setup ================ -->
## Project
[One line: what this project investigates. The full standing brief lives in `docs/project_direction.md`.]

## Stack
[e.g. Python 3.11+, PyTorch, uv, marimo, wandb, ruff. Coding conventions are in the global config; do not restate them here.]

## Commands (prefer `make`, it tracks deps and skips stale-free steps)
[Project make targets and key commands, e.g.:]
- `make help` — list pipeline targets
- `uv sync` · `uv run pytest tests/ -x` · `uv run ruff check .`

## Layout
[Dir map: what lives where.]

## Key docs (read when planning, not auto-loaded)
- `docs/project_direction.md` — the standing research brief (what/why, setup, success criteria, models, datasets, refs). Canonical source of the direction.
- `docs/experiment_log.md` — dated, evidence-forward history. This is the product. Its top line points here at the direction.
- `docs/claims_status.md` — claims ledger: claim -> evidence -> location in the tree.
- `docs/failure_log.md` — what failed and why.
<!-- ================ END PROJECT-SPECIFIC ================ -->

