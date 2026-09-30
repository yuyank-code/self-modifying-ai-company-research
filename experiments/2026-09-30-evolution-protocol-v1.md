# Evolution Protocol V1 — Experimental Plan

Date: 2026-09-30

## Research question
Can one evolution protocol safely improve different agent surfaces while preventing the evolving system from weakening evaluation, expanding authority, or overfitting the selection benchmark?

## Tracks
A. Skill/prompt evolution — mutate external skills/prompts; freeze evaluator, authority policy, model and tools.
B. Coding/harness evolution — mutate control flow, context management, routing and task-completion logic; freeze evaluation and production authority.
C. Replay/search-policy evolution — mutate candidate selection, exploration and budget allocation; freeze target agent and evaluator.

## Required splits
1. evolution/selection set
2. held-out in-domain set
3. cross-domain OOD set
4. security/adversarial set

The final evaluation set is never exposed during evolution.

## Baselines
- no evolution
- single-shot self-refinement
- unrestricted harness evolution
- scope-limited evolution
- scope-limited + regularized evolution
- human-designed baseline where available

## Required measurements
Trial count, seeds, candidate count, evaluator version, confidence intervals, effect size, compute cost, rejected candidates and rejection reasons.

## Security tests
Attempt evaluator edits, acceptance-threshold edits, permission expansion, undeclared-file access, undeclared network access, scope expansion and undeclared persistence. Expected result: external policy rejects each prohibited action.

## Success criterion
Improvement must survive frozen evaluation, held-out evaluation, OOD evaluation, security evaluation, cost accounting and rollback/replay checks. In-distribution score gain alone is insufficient.

## Metrics
Verified Improvement Cost (VIC) = total evolution compute / accepted verified improvements.

Verified Improvement Yield (VIY) = accepted verified improvements / evolution compute.

Evolution Integrity Rate (EIR) = candidates satisfying scope/evidence/authority invariants / total candidates.

OOD Retention = held-out/OOD improvement relative to evolution-set improvement.

Authority Violation Rate = unauthorized attempts / candidate executions.

Rollback Integrity = correctly reverted deployments / deployments requiring rollback.

## Immediate experiments
E105: independent replication of recent harness-evolution systems.
E106: Evolver/Solver decoupling.
E107: fixed vs adaptive evolution budgets.
E108: evaluator-independence sensitivity.
E109: scope-inflation attack.
E110: cross-provider Evolution Commit portability.
E112: Improvement Intelligence transfer.

## Implementation order
1. machine-readable schemas
2. immutable candidate snapshots
3. sandbox
4. evaluator registry
5. external acceptance policy
6. EvolutionCommit lineage
7. canary/rollback simulation
8. Track A, then B, then C
