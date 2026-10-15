# Platform Economics and Company Hypothesis — 2026-10-15

## 6. The NVIDIA Dynamo lesson

NVIDIA Dynamo v1.4.0 (GA 2026-08-14) reports 127 contributors, including 29 first-time contributors, and work spanning routing, multi-datacenter indexing, observability, simulation, Kubernetes rollout controls and compatibility with multiple inference engines. Official release notes: https://docs.nvidia.com/dynamo/dev/reference/releases/v1-4-0. Contributor counts show ecosystem activity, not profitability or market share.

The useful CUDA analogy is not “publish an SDK.” Durable infrastructure platforms need a stable interface, libraries, tools, production integrations, documentation, performance economics and a feedback loop between workloads and engineering. For agent evolution, a platform layer might standardize a candidate/change artifact, verification interface, promotion semantics, lineage, runtime adapters and policy interface. Ecosystem strength becomes credible only when external teams build and maintain adapters/verifiers without founder involvement.

## 7. Working product architecture

Control loop: observe → localize cause → propose bounded diff → static/policy checks → isolated execution → independent hidden evaluation → confidence/cost gate → canary → monitor → rollback or retain.

The proposer must not edit hidden acceptance tests, policy rules, promotion thresholds or production credentials. A separate gate evaluates the candidate; a separate deployment controller applies an approved artifact.

Persist candidate parent/diff, model and tool versions, runtime/dependency hashes, task/test versions, per-task outcomes, grader disagreement, cost/tokens/latency, policy events, confidence interval, approval identity, deployment version and rollback reason.

Avoid one scalar score that can trade away a critical safety property. Use hard policy constraints plus a performance objective, regression requirements, total-cost accounting and uncertainty-aware acceptance. Zero observed failures in a small sample is not proof of safety; deterministic controls and threat modeling remain necessary.

## 8. Company wedge: change control, not magic self-rewrite

Test a vendor-neutral change-control and evidence plane for one deployed agent workflow. The buyer should already be paying to maintain an agent and should have observable failures, regressions or policy-sensitive actions. Choose one vertical only after discovery.

MVP: import one agent and its manifest; collect 20–50 representative tasks and known failures; add deterministic checks for verifiable outcomes; record authorization decisions; represent changes as reviewable diffs; evaluate candidate and incumbent under matched conditions; report effect size, uncertainty, cost and regressions; add canary/rollback after offline evaluation is trustworthy.

Treat pricing as an experiment. Offer a fixed-scope paid pilot with agreed baseline and acceptance criteria. Measure paid conversion, integration time, operator time saved, incidents caught, false-promotion rate and support burden. Positive interviews are not willingness to pay.

Potential moat: labeled failure cases, provenance-rich evaluation histories, portable integrations, domain verifiers and calibration between offline changes and production outcomes. The loop itself is unlikely to be defensible. Switching costs should come from trusted workflows and accumulated evidence, not opacity.

## Stage gates
1. Pain: several buyers report the same costly failure and can share representative tasks.
2. Measurement: outcomes can be evaluated without the agent grading itself.
3. Utility: held-out gains exceed search/evaluation cost.
4. Safety: external policy enforcement blocks prohibited actions.
5. Adoption: a customer repeatedly uses the gate and pays to continue.
6. Platform: third parties can add adapters/verifiers without core changes.

## New experiments — designed, not run

**E189 — Cross-vendor gate:** Compare incumbent, prompt-only repair, bounded multi-layer repair, and bounded repair plus independent acceptance. Hold budgets and tasks fixed; use hidden tasks and repeated trials. Measure held-out success, retained-pass rate, policy violations, false promotion, cost, latency and rollback success.

**E190 — Skill selection:** Compare no skill, curated skill, generated skill, conventional composition and compatibility-checked composition. Pin versions and reserve unseen tasks. Measure pass delta, cost, token overhead, per-task regressions and compatibility errors.

**E191 — Tool integrity:** In an isolated synthetic harness, compare model-only instruction with external authorization, manifest pinning and review. Measure unauthorized-action rate, legitimate completion, false blocks and audit completeness. Do not use real credentials or production targets.

**E192 — Isolation preflight:** Verify network routes, allowed destinations, credential availability, filesystem mounts and logs before each run. Fail closed on any boundary mismatch.

**E193 — Portable artifact:** Apply the same change/evidence schema to two agent runtimes. Measure integration time, missing semantics, reproducibility and rollback reconstruction.

**E194 — Evaluator dependence:** Compare aggregate model scores with deterministic assertions, independent graders and composite policy/performance gates. Measure false acceptance, false rejection, grader disagreement and cost. Distinct model identity alone does not prove independent errors.

All experiments require pre-registered hypotheses/stopping rules, pinned artifacts, hidden tasks, complete reporting and total-cost accounting. No results are claimed until run.