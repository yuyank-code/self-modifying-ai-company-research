# Research Note — 2026-10-17
# Skill Evolution, Recursive Harness Improvement, and Evidence-Gated Promotion

## 1. Executive finding

Two recent papers provide concrete evidence about agent self-improvement at different layers. AIDE² evolves the harness/search algorithm around a fixed model; SkillNEO evolves task-specific procedural artifacts such as instructions, scripts, and resources. Their shared lesson is architectural: the system proposing or exploring modifications should not be the sole authority deciding what becomes the production incumbent.

This is bounded empirical progress, not evidence of unrestricted recursive self-improvement.

## 2. AIDE²: what the experiment shows

Primary source: https://arxiv.org/abs/2609.26457
First-party explanation: https://www.weco.ai/blog/first-evidence-of-recursive-self-improvement

AIDE² uses two nested loops. An outer agent edits the code of an inner agent; the inner agent optimizes code against measurable objectives. The outer loop accepts a rewrite only when the revised agent's grade improves. Selection tasks span machine-learning engineering, heuristic algorithm engineering, and agent-harness engineering.

The paper describes a 100-node trajectory over eight days: 99 rewrite proposals, seven accepted changes, and a selection grade rising from 0.703 to 0.778. Two additional full runs reportedly accepted two and four changes. Candidates are assessed under fixed task-specific budgets, so gains should reflect better use of budget rather than simply more inference or execution spending.

The paper reports positive transfer to ALE-Bench, MLE-Bench, FML-Bench, and WeatherBench 2, the latter treated as out-of-distribution. In the paper's stated protocol, reward hacking on a separate kernel-engineering evaluation fell from 55% to 32%.

### Mechanisms that survived selection

- Bandit-like allocation of search effort across strategy lineages.
- Periodic forking from the best candidate when a lineage stalls.
- Compact, role-specific context rather than concatenating the full search history.
- Conditional failure memory.
- Basic guards intended to discourage overfitting and suspicious outputs.

The paper reports large prompt-size reductions, enabling more search steps under a fixed budget. This suggests practical improvement can come from orchestration, context management, search allocation, and verification without changing model weights.

### Negative results and limits

Many more sophisticated proposals—such as island populations, LLM-judge tournaments, variance-adaptive exploration, and optimizer-curse corrections—failed the acceptance gate or were within noise. The authors also report a maintainability problem: the evolved harness became complex, difficult to understand, and contained dead code. A statistical safeguard intended to reject suspicious outliers had a bug and no effect in the final version.

The “ignition” experiment asks whether an evolved inner agent can improve the next generation more effectively as the outer-loop optimizer. It used only three seeds per arm and is explicitly inconclusive. It does not establish an accelerating self-improvement process.

A source-version discrepancy must be preserved: Weco's July 14 blog reports kernel reward hacking falling from 63% to 34%; the September arXiv report states 55% to 32% under its described protocol. The checked sources do not fully reconcile the figures, so they must not be treated as the same statistic.

The public repository found in this review is the AIDE ML baseline: https://github.com/WecoAI/aideml. A public, turnkey reproduction of the complete AIDE² nested experiment was not found in the sources checked. The base implementation is useful for study but is not the full recursive experiment.

Evidence classification:
- Demonstrated in the authors' setup: multiple accepted harness rewrites under fixed budgets and positive performance on external benchmarks.
- Promising: search-policy and context-management changes can generalize beyond the tasks used to select them.
- Not established: ignition, asymptotically increasing improvement rate, universal transfer, or safe unattended production deployment.
- Important negatives: high proposal rejection, non-monotonic checkpoint rankings, an ineffective buggy safeguard, and reduced maintainability.

## 3. SkillNEO: non-elitist search for task-specific skill bundles

Primary source: https://arxiv.org/abs/2610.20926
Full text and limitations: https://arxiv.org/html/2610.20926v1

SkillNEO distinguishes the current skill bundle considered ready to deploy for a task from candidate versions eligible to serve as parents for further edits. It keeps execution traces with each version, compares parent/child and sibling executions, diagnoses unresolved failures, and can revisit earlier branches using evidence collected later. Bundles can include natural-language instructions, executable scripts, and supporting resources.

This addresses a weakness of greedy or elitist search. A revision may fix one important step while leaving another incomplete, so its overall reward may stay flat or fall. Discarding every candidate that does not immediately beat the incumbent can erase a useful partial repair.

### Results and exact scope

Across 86 SkillsBench tasks in eight broad domains, average reward for GPT-5.4 Mini rose from 49.1% to 66.4%, a gain of 17.3 percentage points. Forty-one task-specific bundles changed; 45 remained byte-identical. Frozen evolved bundles also improved stronger executors on the same task instances: the reported gains on the 41 modified tasks are 10.6 points for GPT-5.5 and 10.5 points for GPT-5.6 Luna.

Search is not free. The method uses up to five generations, proposes six children per generation, promotes up to three for repeated evaluation, and uses three execution repeats for measured comparisons. Across the 41 modified-task histories, the paper reports 1,283 candidate entries (971 retained candidates and 312 duplicate entries removed). These counts establish nontrivial search/evaluation overhead; they are not a direct dollar-cost estimate.

### Central limitation

Development and final evaluation use the same task and input instances. The authors explicitly say that fresh executions on those instances do not establish generalization to unseen tasks or inputs. Cross-model transfer is encouraging, but cross-model transfer on the same instances is not cross-task generalization.

The experiment evolves one bundle per task. It does not yet establish that procedures can be consolidated into a shared skill library and remain beneficial across many workflows without harmful interactions.

Some revised procedures passed local schema or feasibility checks but failed domain-specific requirements such as cost optimality or directory-clearance rules. Local checks can be incomplete even when the system uses executable verifiers.

Evidence classification:
- Demonstrated in the reported setup: non-elitist skill development can improve reward on the tasks used for evolution; some frozen bundles transfer across models on those same tasks.
- Promising: preserving lower-scoring intermediate candidates can enable multi-step repairs; cross-branch procedure reuse may help.
- Not established: generalization to unseen instances, broadly reusable shared skills, or cost-effective production improvement across new tasks.
- Negative evidence: some revisions preserve hidden domain mistakes; candidate search incurs substantial execution overhead; matched-budget ablations are still needed to isolate components.

## 4. Synthesis: two state machines, not one

### Development state machine
- Keep multiple candidate branches.
- Record parentage, edit intent, traces, failures, and costs.
- Permit a candidate to remain eligible for repair even if its score is below the incumbent.
- Use archive revisiting and branch comparison to recover useful partial procedures.
- Apply resource budgets and stop conditions.

### Promotion state machine
- Keep the incumbent unchanged unless the candidate passes predefined acceptance criteria.
- Use fresh replicated evaluations with matched model, task, environment, and verifier settings.
- Require policy compliance independently of aggregate reward.
- Evaluate on untouched input variants after candidate selection is frozen.
- Preserve a final test set the search loop cannot query repeatedly.
- Reject or review changes that are not reproducible, introduce policy violations, or create unacceptable maintenance complexity.

The design principle is **non-elitist exploration, conservative promotion**. Non-elitism is useful for learning; it is not a deployment policy.

## 5. Evaluation architecture

A trustworthy agent-change record should distinguish four signals:

1. Development signal: cheap feedback used by the candidate generator; it may be noisy or gameable.
2. Selection/validation signal: a more robust but still budgeted comparison between candidates and incumbent.
3. Untouched generalization signal: fresh task/input variants used only after the selected artifact is frozen.
4. Policy gate: hard constraints for permissions, consent, prohibited side effects, and environment containment. Policy failures cannot be offset by higher task reward.

Add fixed compute and tool-call budgets; task-level paired differences; uncertainty estimates; regression tests on previously solved tasks; cost per accepted improvement; cost per point of held-out gain; candidate acceptance rate; time-to-improvement; maintainability checks; and an explicit record of evaluator/verifier versions.

Repeatedly exposing aggregate validation scores to an outer optimizer can create adaptive overfitting even when individual test examples are hidden. Reserve an untouched final set and measure transfer on new inputs/tasks.

## 6. Product implication

The research supports a narrow hypothesis: an evidence-first CI gate for changes to agent prompts, skills, routing, workflows, and code. It should not claim that an agent is generally “self-improved.” It should issue a scoped evidence statement about a specific artifact under a specific evaluation envelope: which tasks and variants were tested, budgets used, policy boundaries applied, improvements, regressions, and what was not tested.

A potentially valuable asset is an evolution ledger that makes candidate lineage and promotion evidence portable across agent frameworks. But this is only a wedge if it adds measurable value beyond existing CI, tracing, runtime policy, and evaluation products. Comparative tests and customer interviews remain mandatory.

## 7. Cognitive Holobiont boundary

These methods may eventually contribute infrastructure for adaptive memory, skill accumulation, and coordination across agents. They do not validate the broader Cognitive Holobiont hypothesis. Build demonstrable bounded engineering capability first and keep speculative systems-level claims separate.

## References

- AIDE² technical report: https://arxiv.org/abs/2609.26457
- Weco AIDE² discussion: https://www.weco.ai/blog/first-evidence-of-recursive-self-improvement
- AIDE ML public baseline: https://github.com/WecoAI/aideml
- SkillNEO: https://arxiv.org/abs/2610.20926
- SkillNEO limitations: https://arxiv.org/html/2610.20926v1
- ST-WebAgentBench: https://d3ccyth396mz21.cloudfront.net/publications/st-webagentbench-a-benchmark-for-evaluating-safety-and-trustworthiness-in-web-agents--1
- Agent evaluation survey (ACL Findings 2026): https://aclanthology.org/2026.findings-acl.1330/
- Trace-based assurance framework: https://arxiv.org/abs/2603.18096
