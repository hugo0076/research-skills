---
name: experiment-log
description: Conventions for maintaining docs/experiment_log.md and docs/claims_status.md. Load whenever recording an experiment, a result, an observation, or one of Hugo's decisions.
---

# experiment-log

The direction is **not** here. The standing research brief lives in `docs/project_direction.md` (written during the briefing); this log is dated history and its top line just points at the direction. Do not copy the direction in.

## docs/experiment_log.md — this is your product
It is the thing you will be measured by. Take pride in it: keep it correct, rigorous, and clean.

- **Top line:** `Direction: ./project_direction.md`, then dated history below.
- **Structure:** one section per day (date heading), with free-form subheadings under it. No fixed template; write what the work needs.
- **Evidence-forward:** record what was tried and what was observed. Observation over interpretation. Add interpretation only when it is clearly obvious, or flag it as tentative. Do not spin up elaborate hypotheses that read more into the evidence than it supports.
- **Reproducibility:** you do not need git commits or slurm job IDs by default. Include them for important results if useful. When you produce a plot, link its path. Keep it lean otherwise; a few extra tokens are fine, but do not clutter.
- **Hugo's adjudications:** when Hugo makes a call ("I think this is right, not that"), record it with a `Hugo:` tag. His judgments are authoritative, and exporting them is the whole point of this system, so treat them as load-bearing. Adjudications are the one place interpretation is always welcome.
- **Bar for entry:** nothing lands as established unless it has been rigorously verified or Hugo has adjudicated it. Your own interpretations are fine when clearly correct or clearly flagged tentative; never let speculation look settled.

## docs/claims_status.md — the paper skeleton
Shorter than the log, updated continuously. A paper-writing pass reads this later, so keep it clean.

- Each claim is a heading: a statement, e.g. *"Models have metacognitive access to their own internal states."*
- Under it, the evidence: e.g. *"asked to make its activations accord with a linear direction, the model shifts by Cohen's d ≈ 3,"* with a link to the relevant plot or experiment path.
- A confidence flag. This is **your (the model's) confidence by default**. Only mark it as Hugo's if he has stated a value himself, and say whose it is.
