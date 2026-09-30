# Research Addendum — 2026-09-30

MCP is an execution and authority boundary, not merely a tool protocol. The current specification treats tools as potentially arbitrary code execution paths and requires consent and authorization controls. The platform should therefore place its Authority Plane above MCP and record tool/server identity, capability, resource audience, actor identity, policy decision, provenance and side-effect class.

New threat-model inputs are two 2026 MCP issue reports covering possible public-cache poisoning and server-controlled instruction injection. These are issue reports rather than independent validation, so confidence is medium-low, but both justify adversarial testing.

CUDA lesson: a durable platform moat is an ecosystem, not a single runtime. NVIDIA combines libraries, frameworks, enterprise software, infrastructure, partner hardware and distribution. An adaptive-software equivalent therefore needs portable Artifact, Evolution, Evidence, Authority, Evolution-Commit, Replay, Deployment and Distribution contracts.

E113: freeze model, evaluator, authorization policy and MCP servers; evolve only routing/context management; test malicious tool descriptions, poisoned resources and cross-tenant requests. Measure task success, verified improvement yield, unauthorized calls, privilege escalation, injection susceptibility, cost and rollback integrity.

Commercial implication: prioritize a trusted adaptive-runtime control plane for evidence, safe evolution, authorization, rollback and cost accounting across existing agents, models and tools.
