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
