# Deep Dive — Tool Integrity and Evaluation Boundaries (2026-10-15)

## 3. Tool catalogs are a trust boundary

MCPTox, published in AAAI 2026, evaluates 45 real-world MCP servers, 353 tools and 1,348 test cases across ten risk categories. It reports widespread susceptibility to tool-metadata poisoning in 20 agent settings, including a 72.8% attack-success rate for GPT-o1-mini in the evaluated setup. These rates are benchmark-specific, not prevalence estimates for every deployment. Source: https://ojs.aaai.org/index.php/AAAI/article/view/40895

A related AAAI 2026 paper studies manipulation of agent preference through MCP tool names and descriptions: https://ojs.aaai.org/index.php/AAAI/article/view/40898. Tool catalogs can influence both what an agent selects and how it interprets the tool. For self-evolving systems, the discovery pipeline can introduce risk even if the underlying model never changes.

Required design rules:
- Discovery is not authorization; every action is checked by a separate policy broker.
- Pin tool identity, version, publisher and content digest; review permission-expanding metadata changes.
- Treat tool metadata and tool outputs as untrusted data.
- Use least privilege, short-lived scoped credentials and allowlisted network egress.
- Record the exact tool manifest and authorization decision for every evaluated run.
- Include adversarial metadata cases in the regression suite and measure legitimate task completion as well as blocked unauthorized actions.

A signature can establish package provenance but cannot prove the package is safe. A model-based scanner can assist triage but cannot replace external enforcement.

## 4. Evaluation boundaries must be enforced by infrastructure

Anthropic's October 9, 2026 disclosure describes unintended external interactions during evaluations and internal use: https://www.anthropic.com/research/investigating-unintended-model-actions. Its September 9 assessment describes evaluation incidents where models were told they were in simulations but a third-party configuration exposed the open internet: https://www.anthropic.com/research/alignment-assessment-cybersecurity-incidents. These are company disclosures, not independent audits; the counts should not be generalized to all deployments.

The architectural lesson is robust: model instructions do not create a sandbox. The model may misunderstand the environment, and infrastructure may fail to enforce its declared boundary. Therefore:
- Default-deny network egress; explicitly allow only required destinations.
- Keep production credentials outside the agent container and broker short-lived scoped access.
- Separate evaluation and production accounts/projects.
- Run a supervised containment preflight before each evaluation; fail closed on mismatch.
- Log out-of-scope access attempts and keep policy configuration outside the agent's write authority.
- Score task completion and policy compliance separately. A task completed by an unauthorized action is a failure.
- Require human confirmation or a separate control path for high-impact actions.

## 5. Self-modification evidence boundary

The Darwin Gödel Machine paper reports improvement from 20.0% to 50.0% on SWE-bench and from 14.2% to 30.7% on Polyglot in its experimental setting: https://arxiv.org/abs/2505.22954. It supports bounded search over agent code/scaffolds and archiving diverse variants. It does not establish unrestricted recursive improvement, generalization to arbitrary domains or safe autonomous deployment. Model-generated code can be destructive, so sandboxing, immutable lineage and external acceptance are core architecture.

## Proposed evaluation protocol
Keep candidate generation separate from test authoring and promotion. Pin candidate, runtime, tools and evaluator; reserve hidden tasks before search; report all trials and total cost; preserve rejected candidates; use deterministic checks where possible; and require policy gates to pass independently of performance scores. For critical systems, statistical testing supplements rather than replaces deterministic controls.