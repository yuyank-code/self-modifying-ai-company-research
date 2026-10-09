# Experiments — Claim-Aware Evidence Allocation — 2026-10-09

**Status:** designed, not executed. No results are claimed.

## E159 — Self-preference vs independent acceptance
Compare no evolution, self-preference-only selection, self-preference shortlist plus independent hidden acceptance, and external grading for both search and acceptance. Freeze model, tasks, runtime, candidate budget and dollar budget. Measure search cost, held-out pass rate, pass^N, OOD retention, candidate diversity and false acceptance. Falsify the claim that self-preference is safe for acceptance if its gains disappear on hidden tasks or false acceptance rises.

## E160 — Claim-aware evidence budget planner
Under equal rollout/cost budgets compare full-suite evaluation, random subsets, mid-difficulty tasks (historical pass rates 30–70%) for ranking only, balanced task×scaffold×seed samples, and adaptive allocation based on estimated variance and decision risk. Measure rank correlation, absolute-score error, promotion error, false acceptance/rejection and cost per correct promotion. Screening subsets must never be the sole production acceptance evidence.

## E161 — Scaffold and scorer audit
Compare normal agent runs with de-scaffolded execution where feasible, seeded ground-truth state assertions, rubric scoring and model-based scoring. Report measurement object, scaffold ownership, scorer disagreement, worst-decile performance and tail risk.

## E162 — Policy-constrained web success
Create web-agent tasks with explicit consent, data disclosure, destructive-action and domain-boundary rules. Report nominal completion separately from Completion-under-Policy. Any critical policy breach blocks promotion regardless of average task score.

## E163 — RRSI reproduction and ablation
Use https://github.com/google-research/rrsi as a reference implementation after pinning environments and model access. Compare baseline harness, full regularized evolution, no history-aware exploration, no leakage critic, no noise-adjusted floor and no cost rule/pruning. Evaluate on an evolution split plus untouched OOD tasks. Record all costs, token counts, edits and failures. The repository documents Claude Opus 4.8 via Vertex AI for search roles and Gemini 3.5 Flash as judge in one domain; substitutions must be explicit.

## E164 — Correlated evaluator failure
Inject scorer weaknesses that reward superficial formatting or task-specific shortcuts. Compare same-model self-judge, a distinct-model judge, deterministic assertions and composite acceptance. Measure false acceptance and evaluator error correlation. Distinct model identity alone does not prove statistical independence.

## E165 — Sequential stopping
Compare fixed-size evaluation with a pre-registered sequential test using confidence boundaries/alpha spending. Use null candidates and small true improvements. Measure cost and false acceptance. Do not repeatedly inspect results and stop only when a favorable threshold appears.

## E166 — Production-value calibration
Compare predicted quality/cost/latency improvements with canary and production outcomes. Measure predicted-vs-observed savings, rollback rate, regression severity, time-to-detect and net value after search, verification, canary and rollback costs.

## Shared acceptance rules
- Pin and digest candidate, model, harness, tools, runtime, evaluator and benchmark.
- Reserve hidden tasks before search.
- Report pass@1 and pass^N separately.
- Report nominal success and policy-compliant success separately.
- Include full evolution cost.
- Preserve rejected candidates and negative results.
- Promote only after independent acceptance and required policy/security gates.