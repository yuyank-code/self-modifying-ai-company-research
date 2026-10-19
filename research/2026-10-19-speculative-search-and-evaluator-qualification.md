# Technical Research — 2026-10-19
## Speculative Search, Dependency Coupling, and Evaluator Qualification

### 1. SpecForge: optimize behavioral specifications
Primary source: https://arxiv.org/abs/2610.15708
SpecForge treats natural-language behavioral specifications as a mutable external knowledge layer. The system generates implementations from a specification bundle, evaluates them with execution-validated tests, and refines specifications based on failure traces. It identifies:
- Dependency coupling: a caller's correctness depends on callee behavior.
- Generation coupling: because the model conditions on the whole specification bundle, editing one specification can change code generated for unrelated entities.
Its solution combines a Pareto archive of non-dominated specification states, dependency-constrained entity selection based on the call graph, and expansion from diverse subsets of failures.

The key product lesson is to model a change's dependency surface, not merely its local score. A local improvement can break a dependent component; a single scalar score hides this.
Limits: search procedure is fixed; reference implementation required; main loop evaluates 20 Python modules; tests derived from a reference implementation are only as sound as the test-generation/validation method. This is bounded specification optimization.

### 2. SpecRSI: decouple drafting from acceptance
Primary source: https://arxiv.org/abs/2610.16491
SpecRSI allows successors to be drafted before predecessor evaluations fully finish, increasing search parallelism under finite budgets. Verification-Aware Tree Search (VATS) chooses between additional edits and verification work according to predicted net verified return. Group Verification Optimization (GVO) updates the improver from completed searches while holding the verification contract fixed.

Architecture rule: predictions may schedule work; they must not be promoted to evidence. Only complete, admissible, provenance-scoped verification records authorize adoption.
The paper reports improved terminal gains and a lower cost index in its tested settings. However, parallelism can reduce wall-clock latency while increasing total work, and additional plans can raise raw score but lower net return. Verification batching is condition-dependent rather than universally cheaper.

Suggested reproduction: matched task seeds, same candidate budget, same total token/compute budget, same verification contract; compare sequential full-check, speculative draft + full-check, and speculative draft + adaptive check allocation. Report terminal held-out score, area under verified improvement curve, wall time, total compute, cost per accepted gain, and false promotion.

### 3. AGO: qualify the judge before trusting it
Primary source: https://arxiv.org/abs/2610.01218
AGO's core contribution is to treat evaluation as a decision process rather than score generation. It separates deterministic checks, local guardrails, LLM judging, meta-evaluation, uncertainty estimation, and release policy.
The reported judge results show why this matters: a low-cost judge can produce flawless structured outputs while discriminating near chance; even a stronger judge can be unreliable on particular domains. The paper's gate study shows the trade-off between lower unsafe promotion and more review/block decisions.
Design requirements:
- Judge qualification is versioned by domain, task, policy, and threshold.
- Maintain independent human-labelled or otherwise trusted reference sets.
- Measure discrimination, calibration, confidence intervals, and operating-point metrics.
- Use a four-state decision: PROMOTE, REVISE, BLOCK, or INSUFFICIENT EVIDENCE / REVIEW.
- Track false promotion, false rejection, review load, and improvement throughput together.
- Do not infer evaluator reliability from valid JSON, coherent explanations, or internal confidence.
- Requalify after model, prompt, policy, domain, or data distribution changes.

### 4. Proposed change-evidence artifact
Minimum schema:
- candidate_id, parent_candidate_id, immutable source/config hash
- modification_layer and declared intent
- runtime/model/tool/dependency/policy versions
- dependency graph or declared affected surface
- evaluation contract and hidden holdout version reference
- test results with per-case outcomes
- judge qualification profile and uncertainty
- policy violations (hard veto)
- resource budget, wall time, tokens, compute and estimated cost
- provenance of each result (actual execution vs prediction/cache)
- decision, decision rationale, approver, and rollback target

The artifact should distinguish predicted performance from measured performance. Cached evidence is reusable only if the candidate, environment, policy, evaluator, and scope match the cache key.

### 5. Research judgment
Confidence is medium that the architecture patterns are useful within bounded settings; low that any single paper establishes a universal method. The strongest shared idea is not a specific optimizer but a clean separation between exploration and evidence-backed acceptance.
