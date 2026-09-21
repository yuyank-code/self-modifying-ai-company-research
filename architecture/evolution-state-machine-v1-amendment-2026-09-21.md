# Common Evolution State Machine v1 — Amendment 2026-09-21

This amendment records protocol changes that should be merged into the main draft when the schema is next revised.

## Required changes

1. Insert `ReferenceTrajectories` between `EvolutionScope` and `Operator`.
2. Insert `QualityGate` between `RuntimeEvidence` and `Evaluation`.
3. Add `module_boundaries` and explicit scope breadth to `EvolutionScope`.
4. Add `supported_scope_classes` to `Operator`.
5. Add evaluator independence class to `Evaluation`.
6. Add `minimum_non_regression_requirements` to `Acceptance`.
7. Add hard invariants separating QualityGate from protected Evaluation.
8. Add a hard invariant that self-evaluation cannot be the sole production evidence when the mutation can affect evaluator, benchmark, authority or persistent state.

## Rationale

September 2026 work on HarnessEvolve shows the value of reference trajectories, independent execution/evaluation/optimization/gating, quality filtering and retained-batch non-regression. SafeEvolve shows that safety harness and policy co-evolution can be useful but requires a higher-trust mutation class. Theoretical work on safe harness evolution shows that candidate volume cannot compensate for an evaluation bottleneck and that stagnation can result from certification limits rather than absence of improvement opportunities.

## Proposed state flow

AgentArtifact → HarnessSnapshot → EvolutionScope → ReferenceTrajectories → Operator → Candidate → RuntimeEvidence → QualityGate → Evaluation → Delta → Acceptance → EvolutionCommit → Lineage → Canary → ProductionOutcome → ReplayDataset

## Status

Accepted as a research-design amendment. Full merge into the main schema should occur with the next executable prototype revision.
