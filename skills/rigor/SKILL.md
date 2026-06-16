---
name: rigor
description: The playbook for not fooling yourself. Load for any hypothesis-design or result-interpretation step, before trusting a result, and whenever deciding whether a claim is settled.
---

# rigor (work in progress, grown by accretion)

## Creed
- State the hypothesis before you run; design so it can come out either way.
- Treat a positive as suspicious until it survives the break-tests.
- Defer to Hugo on the hardest holes (see Autonomy).

## Meta-patterns
Reason at the level of these patterns, not a fixed checklist. Each will accrue concrete war-stories over time.

1. **Proxy-target gap.** Your metric is a proxy and can come apart from the target you care about. (E.g. a large mean shift / high Cohen's d need not move evasion AUROC, if the lower-mean distribution still sits above the monitor's threshold.)
2. **Does the test interrogate the claim?** Does the experiment actually probe the thing you think it does?
3. **Claim slippage.** Watch that the claim does not quietly shrink into something smaller or easier than what you set out to show, without anyone noticing it has happened.
4. **Metric gaming.** Could the number move for reasons unrelated to the hypothesis?
5. **Confounded comparison.** Is the comparison clean, or is something else varying alongside the thing you are testing?
6. **Leakage / contamination.** Trusted-set contamination, and subtler routes, e.g. synthetic datasets generated from the same distributions.

## The reviewer test (stopping criterion)
Before treating a claim as settled, imagine an unconvinced reviewer of a paper that cited this result for this claim. What is their objection? On the balance of evidence, does the claim survive, or does the reviewer win?

## Break-the-positive
When a positive lands, first work out exactly what it shows, and whether that is necessarily the thing you wanted to show. If there is doubt, run a test that would surface a difference if the result were actually false.
_[WIP: Hugo's full sequence to be added.]_

## Autonomy
You can always run a quick check yourself if it only briefly uses the bench (~30 minutes or less, ideally seconds). For anything larger, flag Hugo first, or run it in parallel and flag him anyway ("got results back, ~90% good, running this extra check in the background, flagging you anyway"). Do not fully trust your own rigor pass until Hugo has okayed it.

## Accretion
When you or Hugo catch a hole this skill missed, do **not** edit this skill. Write the lesson into the **project-specific `CLAUDE.md`** as a convention, e.g. "we do not use X here because it can be confounded by Y; use Z instead." That turns the lesson into a project norm that propagates forward automatically.

## War-stories
_[Grows over time. Hugo to seed.]_
