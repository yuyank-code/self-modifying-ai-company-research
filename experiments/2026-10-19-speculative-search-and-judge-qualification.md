# Experiment Protocols — 2026-10-19
All experiments below are proposals; none was run in this research pass.

## E208 — Speculative successor drafting vs sequential verification
Question: Does drafting candidates before full verification improve held-out result per unit cost?
Arms:
A. Sequential: fully evaluate a candidate before extending it.
B. Speculative: draft successors early, but fully verify before adoption.
C. Adaptive speculation: use a learned/heuristic return estimate to allocate work between drafting and verification.
Controls: identical initial artifact, task set, model, hidden test contract, candidate-generation allowance, total token/compute budget, and final verifier. Run at least five independent seeds if affordable.
Metrics: terminal held-out score, area under verified-improvement curve, wall time, total work, cost per accepted gain, number of rejected candidates, false promotion, and score uncertainty.
Falsification: no meaningful improvement in verified score or latency at matched total cost, or a rise in false promotion.

## E209 — Dependency-aware specification refinement
Question: Does respecting call-graph dependencies reduce regression and oscillation?
Arms: random entity selection; greedy worst-failing entity; dependency-constrained selection; dependency-constrained plus Pareto archive.
Use a small codebase with seeded caller/callee dependencies, held-out tests, and one unrelated module to measure generation coupling.
Metrics: total pass rate, resolved entities, regression count, number of oscillations, candidate evaluations, and held-out pass rate.
Falsification: dependency-aware search provides no improvement beyond extra search budget, or simply shifts failures to unrelated modules.

## E210 — Evaluator qualification and release decisions
Question: Does domain-specific judge qualification reduce unsafe promotion without overwhelming review?
Construct human-labelled reference cases for two distinct task domains. Freeze candidate/judge versions. Compare:
A. fixed threshold with no qualification;
B. validation-set threshold tuning;
C. qualification with confidence intervals and four-state decisions;
D. deterministic checks plus qualified judge plus hard policy veto.
Metrics: AUROC/AUPRC, calibration error, false-promotion rate, false-rejection rate, review/block rate, throughput, confidence-interval coverage, and cost per decision.
Use separate judge-calibration and final evaluation splits; do not tune on the final set.
Falsification: qualified gating fails to improve decision quality at acceptable review/cost, or confidence intervals are systematically miscalibrated.

## E211 — Judge drift and requalification trigger
Question: How often does evaluator reliability change after a model, prompt, domain, or policy update?
Create controlled shifts in input style, policy wording, task mix, and model version. Track reliability decay and compare periodic requalification against drift-triggered requalification.
Metrics: detection delay, false alarms, missed degradation, label budget, and unsafe promotion after drift.
Falsification: drift-triggered checks do not reduce risk or cost relative to a fixed cadence.

## E212 — Candidate identity and cached evidence
Question: When can evidence be safely reused?
Vary one factor at a time: code hash, tool version, runtime version, policy, evaluator prompt, environment image, and test contract.
The system must invalidate evidence when any factor relevant to the claim changes. Measure invalidation accuracy, unnecessary re-runs, and any case where stale evidence is accepted.
Hard requirement: zero acceptance of evidence whose declared scope does not match the candidate and evaluation contract.

## Safety and reporting constraints
Use deterministic mock tools and isolated local environments. No live external actions are needed. Preserve all failed runs. Report all costs, not only successful candidates. Keep the candidate generator unable to modify hidden tests, reference labels, policy vetoes, or acceptance thresholds.
