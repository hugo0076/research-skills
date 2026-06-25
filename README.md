# research-skills

Claude Code config for autonomous-ish ML research on Spartan (and locally).

## Layout
- `templates/` — CLAUDE.md pieces `setup-repo` assembles per project: `head.md` (fillable project block) + a host block (`host-spartan.md` or `host-local.md`) + `role-core.md` (shared role). Host decides the compute model.
- `skills/` — global skills, symlink into `~/.claude/skills/`:
  setup-repo, slurm-dispatch, experiment-log, rigor, catch-up, request-briefing
- `global/CLAUDE.md` — machine-level prefs for `~/.claude/CLAUDE.md`.

Project docs ontology (created per project by setup-repo): `project_direction.md` (canonical direction) · `experiment_log.md` (dated history + pointer) · `claims_status.md` (ledger) · `failure_log.md`.
  (settings.json is NOT here — restore your own from backup; it holds your hooks/permissions/auto-mode.)

## Install on a machine
1. `mkdir -p ~/.claude/skills`
2. Symlink skills:  `for d in "$PWD"/skills/*/; do ln -sfn "$d" ~/.claude/skills/; done`
3. Global prefs (review first): `cp global/CLAUDE.md ~/.claude/CLAUDE.md`
4. New project: from a project dir, ask Claude to run the `setup-repo` skill.

## Notes
- uv lives in `~/.local/bin` (shared across Spartan nodes); job scripts must put it on PATH.
- Orchestrator runs on a parked `sinteractive` (sapphire) inside a login-node tmux. See `skills/slurm-dispatch`.
- rigor war-stories + break-the-positive sequence are stubs to seed over time.
