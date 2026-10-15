# Experiment Protocol — Change Control and Independent Acceptance (2026-10-15)

**Status:** Designed, not executed. No result claims.

## E189 — Cross-vendor change-control gate
Compare: A) incumbent without evolution; B) prompt-only repair; C) bounded multi-layer repair; D) bounded repair with independent acceptance. Hold model, candidate budget, evaluation budget and task split fixed. Use at least 50 representative tasks plus a hidden acceptance split, repeated trials and two runtimes where feasible.

Metrics: held-out success, retained-pass rate, policy violations, false promotion, cost per accepted change, latency, regression severity, rollback success and integration time. Pre-register the minimum meaningful improvement. A policy-gate failure blocks promotion regardless of average score.

## E190 — Skill value and composition
Compare no skill, curated skill, generated skill, conventional multi-skill composition and compatibility-checked composition. Pin repository/tool versions; reserve unseen tasks; include stale and conflicting skills.

Metrics: pass-rate delta and uncertainty, task-level wins/losses, token/cost overhead, regressions, compatibility errors found, false compatibility declarations and time to choose a skill. Falsify the selection hypothesis if it cannot beat simple or random selection on held-out tasks after cost.

## E191 — Tool-catalog integrity
Use only a closed, isolated synthetic harness. Compare model-only instructions against external action authorization, manifest pinning and review. Measure unauthorized-action rate, legitimate completion, false blocks, metadata-change detection and audit completeness. Never use real credentials or production targets.

## E192 — Isolation preflight
Before every run, verify network routes, destination allowlists, credential availability, filesystem mounts, privilege level and event logging. Use disposable test environments for negative checks. Fail closed on any boundary mismatch. Measure detection, false alarms, time-to-detect and audit completeness.

## E193 — Portable change artifact
Define fields for parent version, candidate diff, model/tool/runtime hashes, intent, task split, test version, per-task outcomes, policy decisions, confidence interval, cost, approver, deployment and rollback. Apply the same artifact to two runtimes; measure integration effort, missing semantics, reproducibility and whether a reviewer can reconstruct the promotion decision.

## E194 — Evaluator dependence
Compare a single aggregate score, deterministic assertions, a distinct-model grader and a composite policy/performance gate. Introduce benign scoring defects in a local benchmark and measure false acceptance, false rejection, grader disagreement, task utility and cost. Distinct model identity alone does not establish independent errors.

## Shared controls
- Pre-register hypotheses, minimum effect and stopping rule.
- Freeze candidate, evaluator and policy versions before acceptance.
- Keep hidden tasks unavailable to the optimizer.
- Report every run, including rejected candidates and failures.
- Separate task success from policy compliance.
- Include search, evaluation, deployment and rollback cost.
- Keep the proposer from editing acceptance tests, policy rules or promotion thresholds.
- Preserve artifacts and lineage for reproduction.
