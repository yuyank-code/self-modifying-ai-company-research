# Common Evolution State Machine v1.1

Date: 2026-09-30

## Objective
A reusable protocol for bounded self-improvement while keeping evaluation, authority, provenance and rollback outside the evolving agent's control.

## Core invariant
The evolving system cannot expand its own EvolutionScope, evaluator authority, production authority, persistence authority, communication authority, or resource budget.

## State machine
AgentArtifact -> HarnessSnapshot -> EvolutionScope -> ReferenceTrajectories -> EvolverIdentity + SolverIdentity -> EvolutionBudget -> Candidate -> SandboxExecution -> RuntimeEvidence -> EvaluatorSet -> Evaluation -> DeltaSet -> AcceptancePolicy -> EvolutionCommit -> Lineage -> Canary -> ProductionOutcome -> ReplayDataset

## V1 mutation surfaces
1. Skill/prompt evolution
2. Coding/harness evolution
3. Replay/search-policy evolution

Model weights and evaluator/security-policy mutation remain outside V1.

## Required promotion evidence
A candidate must beat or be statistically non-inferior to its parent, pass frozen regression tests, stay inside authority/resource ceilings, pass security checks, and preserve complete lineage.

## Required invariants
- Evolver/Solver identities are separate.
- Evaluator independence is recorded.
- Budget is declared before search.
- Scope cannot increase from inside a candidate.
- Authority cannot increase from inside a candidate.
- Acceptance evaluator/policy is immutable during acceptance.
- Accepted changes are replayable or explicitly marked non-reproducible.

## Primary failure modes
Benchmark overfitting, evaluator gaming, reward hacking, unseen-task regression, correlated evaluator failure, cost inflation, authority escalation, persistence corruption, tool/prompt injection, provenance loss and rollback failure.

## Research hypothesis
A common protocol can support multiple evolution surfaces if scope, evidence, acceptance and lineage are standardized while mutation operators remain modular.
