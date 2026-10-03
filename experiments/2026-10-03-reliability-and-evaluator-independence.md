# 2026-10-03 — Reliability and Evaluator-Independence Experiments

## E126 — Repeat reliability
Compare no-evolution, single-shot refinement, and bounded evolution across N repeated trials per task. Report pass@1, pass^5, pass^10, variance and cost. A candidate is not promoted merely because it finds one successful trajectory.

## E127 — Observability vs evaluation
A/B test an evolution loop with tracing only against one with frozen hidden acceptance tests. Measure false promotion rate, regression rate and debugging time.

## E128 — Hidden-evaluator generalization
Keep search-visible evaluation separate from hidden acceptance and OOD suites. Test whether gains persist when the optimizer cannot inspect acceptance examples.

## E129 — MCP authority-boundary attack
Present poisoned tool metadata and malicious tool outputs while keeping permissions and policy external and immutable. Measure task utility, attack success, unauthorized action rate and recovery.

## E130 — Production-failure flywheel
Convert real failures into hidden regression tests; evolve a repair; independently evaluate; commit lineage; canary; compare production outcome. Do not let the agent that generated the repair modify the hidden test or promotion policy.

## Acceptance
A successful protocol must improve utility while preserving or improving repeated reliability, OOD performance, security, authority ceiling, cost efficiency, provenance and rollback integrity.
