# Technical Findings

## 2026-09-02 — Adaptive runtime as the likely near-term layer

### Thesis
The practical meaning of self-modification should be decomposed into layers: prompts/instructions, routing, memory, tools, workflows, code, application configuration, model modules/weights, and eventually multi-agent architecture. Commercial systems should begin with bounded, reversible layers and use measured evaluation to decide whether a modification is promoted.

### Why this layer is attractive
Current developer platforms increasingly expose agents as programmable runtimes. OpenAI's Responses API is explicitly positioned as the future direction for building agents. GitHub's Agent HQ direction places multiple agents into the developer workflow. These developments make an independent control/evaluation layer plausible, but they also raise the competitive threat from platform vendors.

### Design requirement
Every self-modification cycle should be represented as:

observe → diagnose → propose → sandbox → evaluate → compare → approve → deploy → monitor → rollback

The system should retain immutable versions of the agent's prior state and make every modification attributable to a reason, evidence, evaluator and deployment event.

### Research question
Does an adaptive runtime produce durable economic value beyond the capabilities that foundation-model providers and coding platforms will eventually bundle? The answer likely depends on proprietary evaluation data, domain-specific workflows, governance, integration depth and accumulated failure/recovery knowledge.

### Security requirement
Self-modification must not imply unrestricted authority. Separate the ability to propose a change from the ability to execute or deploy it. Use least privilege, isolated execution, approval policies, secrets isolation, audit logs, deterministic regression suites where possible and fast rollback.

## 2026-10-15 — Change control, skill selection and trust boundaries

### Competitive constraint
Kore.ai's October 13 Autoloop announcement describes a productized evaluate-diagnose-repair-verify-rollback cycle. Generic agent optimization is a crowded position; test a vendor-neutral change-control plane with independent acceptance, portable provenance and external policy enforcement. Source: https://www.kore.ai/blog/autoloop-continuous-ai-agent-optimization-for-production

### Skill evidence
SkillsBench reports a +16.2 percentage-point average for curated skills across 86 tasks and 11 domains; SWE-Skills-Bench reports +1.2% average across roughly 565 software-engineering task instances and 39 of 49 skills with no pass-rate gain. Do not average these results: task distributions, curation and domain differ. Skills are conditional interventions and need paired held-out tests, compatibility metadata and cost accounting.
Sources: https://arxiv.org/abs/2602.12670 and https://arxiv.org/abs/2603.15401

### Tool and evaluation boundaries
MCPTox and MPMA (AAAI 2026) show that tool metadata can influence agent behavior and tool choice. Tool discovery must not imply authorization. Keep policy, credentials, network egress and promotion control outside the model.
Sources: https://ojs.aaai.org/index.php/AAAI/article/view/40895 and https://ojs.aaai.org/index.php/AAAI/article/view/40898

Anthropic's 2026 disclosures describe unintended external actions during evaluations and environments that failed to enforce intended isolation. This reinforces default-deny egress, scoped credentials, environment preflight and policy-compliant success as a separate gate.
Sources: https://www.anthropic.com/research/investigating-unintended-model-actions and https://www.anthropic.com/research/alignment-assessment-cybersecurity-incidents

### Platform lesson
NVIDIA Dynamo v1.4.0's reported contribution activity and integration breadth illustrate that infrastructure platforms compound through stable interfaces, production integrations, observability, compatibility and contributor ecosystems. An agent-change platform needs similar ecosystem behavior; an SDK by itself is insufficient.
Source: https://docs.nvidia.com/dynamo/dev/reference/releases/v1-4-0


## 2026-10-17 — Recursive harness improvement and non-elitist skill evolution

### AIDE²
The AIDE² paper reports a fixed-budget, two-loop system that rewrites an agent harness. Its main trajectory accepted 7 of 99 rewrites and raised the selection grade from 0.703 to 0.778; two additional runs accepted 2 and 4 rewrites. It reports transfer to ALE-Bench, MLE-Bench, FML-Bench and WeatherBench 2. The separate ignition experiment is explicitly inconclusive. The evolved harness uses bandit-like strategy allocation, forking after stagnation, compact role-specific context, and conditional failure memory. Most proposed changes were rejected; the authors report a buggy/inactive statistical safeguard and increased code complexity. Do not equate harness-level improvement with weight-level or unrestricted recursive self-improvement.
Sources: https://arxiv.org/abs/2609.26457 and https://www.weco.ai/blog/first-evidence-of-recursive-self-improvement

### SkillNEO
SkillNEO separates the current deployment skill from candidates eligible for further edits. Across 86 SkillsBench tasks, GPT-5.4 Mini reward rose from 49.1% to 66.4%, but development and final evaluation used the same task/input instances. Cross-model reuse on those same tasks is promising, not proof of unseen-task transfer. The authors call for unseen-instance evaluation and shared-library research. Some evolved skills still fail domain constraints despite local schema/feasibility checks.
Source: https://arxiv.org/abs/2610.20926

### Design principle
Use non-elitist exploration and conservative promotion. Preserve candidate lineage, traces, edit rationale and failed candidates; keep the deployable incumbent separate. Require matched budgets, fresh replication, untouched test variants, independent policy checks, cost accounting and maintainability assessment before promotion.

### Evaluation risk
Repeatedly exposing aggregate validation scores to an outer optimizer can create adaptive overfitting even when private test examples are hidden. Reserve an untouched final set and measure transfer on new inputs/tasks. Task success and policy compliance must be separate axes; policy violations cannot be compensated by aggregate reward.

### Open questions
- Can non-elitist search beat greedy and elitist baselines under matched budgets?
- Do evolved skills generalize to unseen variants and a shared cross-task library?
- What is the cost per accepted improvement and cost per held-out gain?
- Can existing CI/evaluation products already produce the evidence bundle with equivalent quality?
- Can evolved code remain maintainable enough for production?


## 2026-10-17 addendum — Policy-compliant completion and assurance overlap

ST-WebAgentBench (ICLR 2026) reports 375 tasks with 3,057 policies and proposes Completion under Policy (CuP). Across three evaluated open web agents, average CuP was below two-thirds of nominal completion rate. This is benchmark-specific but supports treating policy compliance as a hard, separate axis rather than folding it into a weighted task score.
Source: https://d3ccyth396mz21.cloudfront.net/publications/st-webagentbench-a-benchmark-for-evaluating-safety-and-trustworthiness-in-web-agents--1

A trace-based assurance paper proposes contract-enriched Message-Action Traces, replay, budgeted counterexample search, fault injection at service/retrieval/memory boundaries, and action mediation. It explicitly describes itself as a methodology whose full empirical prototype evaluation remains future work.
Source: https://arxiv.org/abs/2603.18096

The ACL Findings 2026 agent-evaluation survey identifies cost-efficiency, safety, robustness, and fine-grained scalable evaluation as open problems.
Source: https://aclanthology.org/2026.findings-acl.1330/

Commercial implication: generic traceability/contract checks alone are unlikely to be a defensible wedge. Require a comparative baseline and prove an incremental failure class, lower cost, better reproducibility, or vertical-specific business outcome.
