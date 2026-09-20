# Common Evolution State Machine v1 — Draft

Status: research/prototype specification, 2026-09-20

## Purpose

A vendor-neutral state machine for representing bounded self-improvement across agents, harnesses, skills, workflows, coding systems and future model-level adaptation.

## State flow

`AgentArtifact`
→ `HarnessSnapshot`
→ `EvolutionScope`
→ `Operator`
→ `Candidate`
→ `RuntimeEvidence`
→ `Evaluation`
→ `Delta`
→ `Acceptance`
→ `EvolutionCommit`
→ `Lineage`
→ `Canary`
→ `ProductionOutcome`
→ `ReplayDataset`
→ next improvement-budget/operator decision

## Core objects

### AgentArtifact
Immutable content-addressed reference to the model, prompt/policy bundle, skill set, workflow definition or code artifact being improved.

Required fields:
- artifact_id
- content_hash
- provider/model identity
- dependency manifest
- license/provenance
- creation timestamp

### HarnessSnapshot
The exact tool/MCP/routing/context-management/runtime environment used for the candidate.

Required fields:
- harness_id
- tool manifest
- MCP/server manifest
- model routing policy
- context/memory policy
- environment image/hash
- runtime version

### EvolutionScope
The trust boundary for mutation.

Required fields:
- mutable object classes
- immutable object classes
- permitted operators
- maximum semantic/structural diff
- dependency-change policy
- authority ceiling
- persistence ceiling
- evaluation requirements
- rollback requirements
- compute budget

### Operator
A versioned transformation strategy such as Draft, Improve, Debug, Crossover, skill optimizer, routing optimizer, replay-policy optimizer or code patch generator.

Required fields:
- operator_id/version
- input contract
- output contract
- historical success statistics
- known failure modes
- expected cost

### Candidate
A proposed mutation plus complete parent linkage.

Required fields:
- candidate_id
- parent artifact
- operator
- diff/patch
- dependency delta
- state delta

### RuntimeEvidence
Evidence about what actually executed.

Required fields:
- runtime identity
- environment identity
- tool calls
- data classes touched
- network/process/file events as available
- policy decisions
- timestamps
- attestation/evidence references

This object should remain compatible with open runtime-attestation/compliance ecosystems where possible rather than inventing a closed standard.

### Evaluation
Independent evidence about performance and safety.

Required fields:
- evaluator identity/version
- benchmark/test version
- protected/visible status
- statistical method
- repeated-run results
- regression results
- security results
- contamination/provenance metadata

### Delta
A structured comparison between parent and candidate.

Dimensions:
- PerformanceDelta
- ReliabilityDelta
- CapabilityDelta
- AuthorityDelta
- PersistenceDelta
- ExposureDelta
- EvaluatorDelta
- DependencyDelta
- CostDelta

### Acceptance
Decision produced by a policy engine, not by the mutable agent alone.

Required fields:
- decision
- policy version
- required evaluator quorum
- evidence references
- human approval if required
- reason

### EvolutionCommit
The atomic audit object for an accepted/rejected improvement.

Minimum contents:
- parent artifact hash
- candidate artifact hash
- EvolutionScope
- Operator/version
- RuntimeEvidence references
- Evaluation references
- Delta
- Acceptance
- cost
- deployment pointer
- rollback pointer

### Lineage
Persistent graph of evolutionary relationships between artifacts and commits.

### Canary
Controlled production deployment with explicit duration, traffic fraction, invariants and automatic rollback conditions.

### ProductionOutcome
Observed post-deployment result including business value, regressions, incidents, support burden and realized cost.

### ReplayDataset
Immutable or versioned historical trajectories and environments suitable for offline improvement-policy optimization.

## Hard invariants

1. Mutable agents cannot directly authorize their own irreversible actions.
2. Evaluator changes invalidate dependent claims unless a new protected evaluation is completed.
3. Every promoted mutation must have a complete parent lineage.
4. Authority increases require explicit policy approval.
5. Persistent-state changes must be represented as deltas.
6. Production promotion requires rollback capability.
7. Benchmark/evaluator provenance must be recorded.
8. Candidate generation may be cheap and high-volume; acceptance must be protected.
9. Search, replay and trust compute budgets must be separately observable.
10. An improvement that increases benchmark score while violating authority/provenance/security invariants is not an improvement.

## Minimal API hypothesis

- `POST /evolution/jobs`
- `POST /evolution/candidates`
- `POST /evolution/evaluate`
- `POST /evolution/accept`
- `POST /evolution/commits`
- `POST /deploy/canary`
- `POST /deploy/rollback`
- `GET /evolution/lineage/{id}`
- `GET /evidence/{commit}`
- `GET /deltas/{commit}`
- `POST /replay/datasets`
- `POST /replay/optimize`

## Prototype success criterion

The local prototype is successful if at least three materially different evolution systems can be represented without changing the core object model:

1. prompt/skill evolution
2. coding/harness evolution
3. replay-assisted search-policy evolution

A stronger success criterion is representation of a vendor-native optimizer and an open-source optimizer using the same protocol.

## Open questions

- What is the smallest useful EvolutionScope type system?
- Which runtime evidence standards can be referenced directly?
- How should evaluator quorum work when evaluators disagree?
- How much semantic diff can be measured automatically?
- What constitutes a sufficient proof of non-regression?
- Can EvolutionCommits be portable across model providers?
- What evidence can be made cryptographically verifiable without exposing customer secrets?
- Which objects should be open standards versus proprietary intelligence?
