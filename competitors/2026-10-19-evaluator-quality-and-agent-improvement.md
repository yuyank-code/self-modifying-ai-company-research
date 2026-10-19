# Competitive Note — 2026-10-19
## Market observation
The broad categories of agent observability, evaluation, guardrails, and continuous improvement are already occupied. Official product positioning:
- Arize AX / Phoenix: https://arize.com/ — traces, evaluations, online evaluation, failure analysis, and continual-improvement workflows.
- Galileo: https://galileo.ai/ — evaluation, safety/security checks, failure analysis, and evaluation-to-guardrail workflows.
- Torina: https://www.torina.io/ — agent evaluation tied to business outcomes and recommendations.
These are vendor claims and category descriptions, not independent comparative proof.

## New research competitor: AGO AI Quality Gate
Paper: https://arxiv.org/abs/2610.01218
AGO describes a release gate for RAG systems with a four-state decision model, layered scoring, a beta-binomial regression-risk estimate, and mandatory evaluator meta-evaluation. It reports that a low-cost judge can be barely above chance despite producing valid, plausible explanations; judge performance varies across domains. Its simulated gate results reduce unsafe promotions at the cost of more review/blocking and lower promotion throughput.
This overlaps materially with the proposed evidence-first certification gate. We should not claim invention of evaluator qualification, probabilistic release gates, or four-state decisions.

## Narrower differentiation hypothesis
Potential differentiation must be demonstrated in one of these areas:
1. Change-level evidence across code, prompt, skill, routing, tool, and policy versions.
2. Dependency-aware regression detection for agent modifications.
3. Portable evidence artifacts that work across multiple runtimes and CI systems.
4. Reproducible evaluator qualification and requalification after drift.
5. A measured reduction in review cost or a particular class of unsafe promotion that existing tools miss.

## Product decision
Do not build a generic dashboard first. Prototype the artifact and gate as a CI component. Integrate with existing tracing and runtime controls. Validate with buyer interviews and an apples-to-apples benchmark against ordinary CI plus the customer's current evaluation stack.

## Open questions
- Is the buyer AI platform engineering, application engineering, security, or compliance?
- Is the budget tied to observability, CI/CD, security, or model quality?
- Can a small team integrate the gate in less than one engineer-day?
- What data may be stored in a hosted product versus remaining in the customer's environment?
- Would customers pay for a release decision, or expect it as a feature of their existing platform?
