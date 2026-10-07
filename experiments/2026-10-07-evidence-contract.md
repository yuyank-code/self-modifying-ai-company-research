# Evidence Contract v0.1 — 2026-10-07

## Objective

Determine whether evolution evidence remains trustworthy when model, harness, evaluator, runtime, tools or benchmark versions change independently.

## Core object

EvolutionCommit references immutable digests for parent, candidate, model, harness, tools, runtime, evaluator, benchmark/task set, reference trajectories, authority policy and configuration.

## Status classes

- VALID: all required evidence-bound components match.
- CONDITIONAL: a changed component has a separately verified equivalence relation.
- STALE: a relevant component changed but the result may remain informative.
- INCOMPATIBLE: a required component changed materially.
- UNVERIFIABLE: provenance or evidence is missing.

## Experiments

E145: model-version change and replay.

E146: harness-only change and replay.

E147: same model/tasks across multiple scaffolds; measure score, pass^N, cost and failure modes.

E148: optimize pass^N under fixed search and verification budgets.

E149: execute the same candidate under two security substrates and compare effective authority, not only declared policy.

E150: change benchmark/evaluator versions while retaining the same name; verify automatic invalidation.

E151: test whether failure/repair patterns can transfer through abstract features without exposing tenant artifacts.

## Acceptance criteria

1. No silent reuse of stale evidence.
2. Exact provenance preserved.
3. Capability separated from scaffold effects.
4. Reliability measured under repetition.
5. Full evolution cost reported.
6. Replay/rollback deterministic where technically possible.

## Negative evidence to seek

- version metadata still fails to prevent misleading evidence;
- evaluator equivalence is prohibitively expensive;
- pass^N optimization consumes excessive search cost;
- runtime authority differs despite identical policy declarations;
- cross-tenant abstraction fails to transfer.
