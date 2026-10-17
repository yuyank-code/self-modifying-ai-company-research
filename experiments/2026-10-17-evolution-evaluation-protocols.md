# Experiment Protocols — 2026-10-17
# Evolution Search, Generalization, and Evidence-Gated Promotion

Status: designed only; no runs have been executed.

## E200 — Elitist vs non-elitist skill evolution

Question: Does retaining lower-scoring versions for further repair beat incumbent-only or historical top-k search when budgets are matched?

Setup:
- Use eight benign tasks with deterministic or machine-checkable outcomes: structured file transformation, code repair in a test repository, document extraction, and constrained planning in a toy environment.
- Prepare development inputs, validation inputs, and untouched final variants for each task.
- Compare greedy incumbent-only refinement, elitist historical top-k, and non-elitist parent pools with archive revisiting.
- Hold model, edit operators, initial bundle, verifier, candidate count, and execution/tool-call budget constant.
- Start with two independent search seeds per arm; expand only if the difference warrants the cost.
- Freeze the artifact before testing on untouched variants.

Metrics: untouched-test reward; policy-compliant completion; regressions on previously passing tests; cost per accepted improvement; candidate acceptance rate; useful descendants originating from below-incumbent candidates; repeated-failure rate.

Controls: no final-test feedback to the generator; same task/verifier versions; record every edit, trace, and execution ID; use paired task-level differences and report uncertainty.

Falsification: if non-elitist search does not improve untouched-input performance or cost-normalized results under matched budgets, do not add its search-state complexity to the MVP.

## E201 — Unseen-instance and cross-task transfer

Question: Do evolved bundles generalize to new inputs and related tasks, or primarily fit the development instance?

Setup:
- Select six tasks with at least five independently generated variants each.
- Use variants 1–2 for development, 3 for validation, and 4–5 as untouched test inputs.
- Evolve on development variants only; freeze the selected bundle before final evaluation.
- Where meaningful, test one adjacent task in the same domain and a second model/runtime.
- Keep policy suite and environment fixed.

Metrics: development/validation/test deltas; transfer gap; cross-model delta; regressions on adjacent tasks; tokens, tool calls, latency, and dollars per held-out point gained.

Falsification: if gains collapse on unseen variants or adjacent tasks, position the artifact as task-specific tuning rather than a reusable skill or general improvement.

## E202 — Multi-seed, cost-normalized harness improvement

Question: Can bounded harness optimization produce repeatable gains across independent seeds and external task families?

Setup:
- Study the public AIDE-style baseline at https://github.com/WecoAI/aideml; do not claim this alone reproduces AIDE².
- Optimize only bounded harness components such as candidate selection, context compaction, and failure-memory policy; keep the model fixed within each arm.
- Use a development set with public feedback and a separate selection score.
- Run at least three independent outer-loop seeds in the pilot, expanding to five or more if affordable.
- Freeze checkpoints and test on task families never used during selection.
- Include a human-engineered baseline under the same compute/tool budget.

Metrics: held-out score at equal cost; accepted rewrites; proposal rejection/bug rate; external transfer; proxy-vs-downstream reward-hacking rate; prompt tokens per step; code complexity/dead-code ratio; maintenance time; seed variance.

Controls: disposable local sandbox and synthetic data; evaluation scripts outside the candidate's writable directory; untouched final set; evaluator changes trigger separate review and clean rerun. Do not infer ignition from accepted rewrites; test next-generation optimization across multiple seeds and external evaluations.

Falsification: if gains disappear across seeds, external task families, or fixed-budget comparisons, report local optimization rather than recursive self-improvement.

## E203 — Evolution ledger and promotion-gate ablation

Question: Does a structured evolution ledger plus independent promotion gate detect meaningful failures that simple CI and existing evaluation tools miss?

Conditions:
1. Baseline CI and unit tests.
2. Baseline plus an existing evaluation/trace tool.
3. Baseline plus runtime policy checks.
4. Full gate: candidate lineage, matched-budget comparison, untouched variants, policy gate, evidence bundle, and staged promotion.

Use local mock systems and synthetic data to inject: functional regression; a hidden requirement missing from the verifier; consent/permission violation despite task completion; flaky stochastic score; overfit to development inputs; attempted edits to test files; missing/non-reproducible artifacts; and cost/latency increase that outweighs a score gain.

Metrics: detection rate by fault class; false rejection/acceptance; incremental failures caught beyond baseline tools; independent reconstruction of promotion decision; time to reconstruct; added latency, compute cost, and integration effort.

Falsification: if the full gate adds no important detection beyond existing tools at acceptable cost, abandon the generic gate wedge or narrow it to a vertical-specific gap.

## Proposed decision thresholds

These are hypotheses for pilot governance, not established facts:
- Zero tolerated hard-policy violations in the final acceptance set.
- At least one important incremental failure class detected beyond baseline tools.
- Positive untouched-test improvement with uncertainty reported.
- Cost per accepted improvement measured end-to-end.
- Every promoted artifact reproducible from versioned inputs/toolchain.
- No promotion based solely on an LLM-judge score.
- Human review when the verifier is ambiguous, the environment cannot be attested, or the change expands authority.

## Run-report template

Artifact and commit hashes; task/input split; model/runtime/tools/verifier versions; candidate count and budget; task-level incumbent/candidate outcomes; policy violations and regressions; confidence intervals and seed variation; cost, latency and tokens; failed candidates and rejection reasons; untouched-test result; decision (promote/reject/inconclusive).
