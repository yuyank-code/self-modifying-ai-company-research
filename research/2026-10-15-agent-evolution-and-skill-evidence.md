# Deep Dive — Agent Evolution and Skill Evidence (2026-10-15)

## 1. Kore.ai Autoloop: competitive update
Source: https://www.kore.ai/blog/autoloop-continuous-ai-agent-optimization-for-production (announced 2026-10-13).

Kore.ai describes a loop that traces execution across agent handoffs, diagnoses the construct responsible, applies targeted repairs, checks several goal dimensions, and rolls back or requests human review when a change cannot be verified. It explicitly acknowledges that an optimizer can only address goals represented in its tests and specifications. This is a vendor description, not an independent efficacy study.

Strategic conclusion: generic “agent optimizer” positioning is crowded. A new company needs to prove something harder to bundle: cross-vendor change records, acceptance tests the candidate generator cannot edit, calibrated uncertainty, policy enforcement outside the model, and portable promotion/rollback artifacts. These are hypotheses, not proven moats.

Technical lesson: localize the failure before choosing what to mutate. A routing or contract defect should not automatically trigger a prompt rewrite; otherwise a score can improve while the underlying failure persists.

## 2. SkillsBench vs SWE-Skills-Bench
SkillsBench: https://arxiv.org/abs/2602.12670. It reports 86 tasks across 11 domains and 7,308 trajectories. Curated skills improve mean pass rate by 16.2 percentage points, but 16 of 84 tasks have negative deltas and self-generated skills provide no average benefit. Effects differ by domain.

SWE-Skills-Bench: https://arxiv.org/abs/2603.15401. Across 49 skills and roughly 565 software-engineering task instances, it reports +1.2% average improvement; 39/49 skills have zero pass-rate gain, seven show larger gains and three degrade performance. Some skills increase token use without improving pass rate; stale guidance can conflict with repository context.

SWE-MSB / SkillSpec: https://github.com/ai4se4ai-lab/swe-multiskillsbench. Its repository reports 44 multi-skill tasks and better resolution with a compatibility-checking pipeline than with conventional skill injection. Treat these as repository-reported results pending independent reproduction.

These results should not be averaged: task mix, domain, curation and single-skill versus composition differ. Joint conclusion: a well-matched curated skill can help, but irrelevant or stale skills can do nothing or cause regressions. Self-generated instructions are not automatically useful, and combining skills can introduce compatibility defects.

Product implication: build a versioned skill registry tied to task families, not a generic skill marketplace. Store applicability, prerequisites, compatible tool/runtime versions, expected cost and known failure modes. Keep a skill only when paired held-out evaluation shows its incremental benefit after token, latency and maintenance cost.

## Minimum skill experiment
Pin model, scaffold, repository commit, tool versions and tasks. Compare no skill, curated skill, generated skill and verified composition on matched tasks/seeds. Reserve unseen tasks. Report task-level deltas, confidence intervals, cost, tokens, latency, regressions and policy outcomes. Include stale and conflicting skills. No new benchmark was run in this pass.