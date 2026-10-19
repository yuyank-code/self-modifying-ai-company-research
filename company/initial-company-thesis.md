# Initial Company Thesis

## Objective
Build a company around software that can continuously improve bounded AI systems while providing measurable evaluation, governance and rollback.

## Initial positioning
Do not sell "self-modifying AI" as the primary customer value proposition. Sell continuous improvement, reliability, lower maintenance cost, faster resolution and measurable agent performance. Self-modification is the mechanism underneath.

## Candidate wedge
A strong first wedge is software engineering / DevOps because repositories, tests, deployments and incident outcomes provide measurable feedback. A second candidate is enterprise workflow agents where actions and outcomes are auditable.

## MVP
A versioned adaptive-agent runtime that stores agent configuration and tool policies as versioned artifacts; observes failures and task outcomes; generates candidate changes; executes candidates in isolated sandboxes; runs regression, security and task-level evaluations; compares candidate against incumbent; promotes only if predefined thresholds are met; monitors the deployed version; and rolls back automatically when safety/performance criteria fail.

## First-customer validation
Target teams already spending significant engineering time maintaining internal agents or automation. Offer a fixed-scope pilot with a measurable baseline and a commitment to prove improvement rather than asking customers to buy a futuristic platform vision.

## Longer-term platform
The MVP can expand into agent runtime, evaluation infrastructure, adaptive memory, self-modifying application tooling, governance, simulation, SDK, integrations and eventually an ecosystem/marketplace.

## Strategic moat hypotheses
1. Proprietary evaluation datasets from real agent failures.
2. A library of validated improvement operators.
3. Deep workflow/integration knowledge.
4. Safety and governance infrastructure.
5. Cross-model support that prevents dependence on one foundation model.
6. Developer ecosystem and third-party extensions.
7. Accumulated evidence about which modifications improve which classes of tasks.

These are hypotheses and must be validated rather than assumed.

## Long-term direction
If the company develops reliable adaptive agents, the resulting runtime, memory, evaluation, specialization and coordination layers could provide practical infrastructure for the user's longer-term Cognitive Holobiont research. The relationship should be treated as a research hypothesis, not as a guaranteed outcome.

## Revision — 2026-10-15: Narrow the initial wedge

### Competitive reality
Kore.ai's October 13, 2026 Autoloop announcement makes generic continuous agent optimization an established competitive direction. The initial pitch should not be “we build self-improving agents” or a generic optimizer dashboard. Source: https://www.kore.ai/blog/autoloop-continuous-ai-agent-optimization-for-production

### Updated working hypothesis
Test a vendor-neutral agent change-control and evidence plane. It versions bounded changes to prompts, skills, routing, tools, workflows or code; evaluates candidates against independent held-out tasks and policy checks; records provenance; and supports staged promotion and rollback.

### Differentiation to prove
1. Independent acceptance that the change generator cannot edit.
2. Portable change/evidence artifacts across multiple runtimes.
3. Policy enforcement outside model instructions.
4. Calibrated false-promotion and regression measurements.
5. Accounting for search, evaluation, deployment and rollback costs.

### First validation plan
- Interview 10–15 teams already operating agents; ask about their last three regressions, detection/recovery time, current tools, budget owner and evidence required for a paid pilot.
- Select one workflow with objectively testable outcomes rather than targeting every agent category.
- Run a paid, fixed-scope pilot only after baseline measurement and a policy boundary are agreed.
- Treat pricing as an experiment; do not infer willingness to pay from positive interviews.
- Do not expand to a platform/marketplace until a customer repeatedly uses the gate and external contributors can add an adapter or verifier.

### Updated risks
- Incumbents can bundle optimization, tracing and rollback.
- A vendor-neutral product may lack a buyer if teams accept provider-specific tooling.
- Hidden tests can be incomplete; independent graders can still be correlated or gameable.
- Evaluation itself can create risk if containment is not enforced externally.
- Customer-specific integrations may prevent attractive margins and ecosystem effects.

The original thesis remains a hypothesis. This revision narrows the experiment; it does not declare product-market fit.


## Revision — 2026-10-17: Separate exploration from promotion

### New empirical evidence
AIDE² reports bounded harness-level self-improvement: seven accepted rewrites among 99 proposals in its main eight-day run, improvement on external benchmarks, and an explicitly inconclusive ignition test. SkillNEO reports task-specific skill-bundle gains, but its final evaluation uses the same task/input instances used during development. These results support bounded adaptation, not unrestricted recursive improvement.
Sources: https://arxiv.org/abs/2609.26457 and https://arxiv.org/abs/2610.20926

### Architectural decision
Represent the evolution process as two separate state machines:
- Exploration frontier: can preserve low-scoring candidates that may contain useful partial repairs.
- Deployment incumbent: changes only after a stricter gate based on fresh replicated comparisons, untouched input variants, policy compliance, and maintainability.

The CI product should issue a scoped evidence statement about one artifact under one evaluation envelope, not claim the agent is generally “improved.”

### Product-risk update
SkillNEO's same-instance limitation means a score gain on the evolution task is not enough for a general-purpose certification claim. The product must enforce a frozen artifact and an untouched final evaluation set, and distinguish task-specific tuning from cross-task generalization. Search must account for evaluation budget, failed candidates, and adaptive overfitting.

### Next decision gate
Build the smallest verifier-backed prototype and compare greedy, elitist, and non-elitist search under matched budgets. If unseen-input gains or incremental value over existing CI/evaluation tooling are not demonstrated, narrow or abandon the wedge. Customer willingness to pay remains unvalidated.


## Revision — 2026-10-17 addendum: Generic assurance is not enough

ST-WebAgentBench demonstrates why policy-compliant completion should be measured separately from nominal task success. A trace-based assurance methodology also already proposes contract-carrying traces, replay, fault injection, and action mediation, although it has not yet reported a full empirical prototype study. Therefore a generic evidence ledger or contract schema is not, by itself, a moat.
Sources: https://d3ccyth396mz21.cloudfront.net/publications/st-webagentbench-a-benchmark-for-evaluating-safety-and-trustworthiness-in-web-agents--1 and https://arxiv.org/abs/2603.18096

Differentiation must be demonstrated as incremental value over baseline CI, existing evaluation/observability tools, and runtime-policy controls. Candidate forms include domain-specific verifiers, cross-runtime promotion evidence, lower-cost reproducible regression testing, or a workflow-specific outcome that incumbents do not already provide. If the comparative test shows no meaningful incremental value, change the wedge rather than rebrand generic assurance.


## Revision — 2026-10-19: Diagnosis, search, and evaluator quality

### Revised technical thesis
The near-term capability to test is diagnosis-first, budget-aware improvement with independent promotion evidence. Recent work on SpecForge, SpecRSI, and AGO AI Quality Gate suggests three distinct problems: search over modifications without discarding useful partial candidates; respect dependencies so local fixes do not create regressions; and qualify the evaluator before its score is used for release decisions.

### Candidate product boundary
Keep the first product as a CI-native change-evidence gate for one bounded agent workflow. Integrate existing tracing and runtime controls; do not rebuild generic observability, identity, or sandboxing. The defensible value must be a measured reduction in a specific failure class, lower review cost, or more reliable promotion decisions.

### Mandatory evaluator qualification
Every evaluator/judge should have a versioned qualification profile tied to a domain and decision threshold. Measure discrimination, calibration, confidence intervals, false acceptance, false rejection, and per-domain variation against independent reference labels. Well-formed judge output is not evidence of judge accuracy. When evaluator quality is insufficient, the gate must return REVIEW/INSUFFICIENT-EVIDENCE rather than silently promote.

### Search and change semantics
Maintain a candidate archive separate from the production incumbent. Explore multiple branches and permit speculative drafting, but do not treat predictions or incomplete checks as evidence. Promotion requires complete, scoped, provenance-checked evidence. Track dependencies between specifications, tools, modules, and downstream behavior.

### Updated falsification criteria
Narrow or abandon the certification-gate thesis if a controlled comparison against ordinary CI, existing evaluation/observability products, and runtime policy tools finds no material incremental failure detection or decision-quality improvement at acceptable integration and evaluation cost. Do not claim recursive improvement if only external specifications, prompts, or harnesses change; report the exact modification layer.

### Research status
This addendum is a strategic synthesis of public papers and vendor positioning, not an executed prototype or customer-validation result. No willingness-to-pay evidence has yet been collected.
