# FIELD_MAP_edges.md — the field's future-work questions

> The field's OWN future-work graph as QUESTIONS, separate from the tensor in FIELD_MAP.md.
> The graph is tripartite: paper -> question -> cell. From this TSV one can rebuild ANY
> diagram (by paper, by canonical question (fwq), by target cell). Built into
> backend/content/edges/scheming.json by scripts.field_map.parse_edges. English only
> (31 sourced, 5 greenfield). The future-work graph lives only
> here, not in FIELD_MAP.md; each `source`/`realizers` P-id resolves to a paper in the
> FIELD_MAP.md §2 catalog and to its tensor points in the map.

## Schema (TSV, tab-delimited)

A QUESTION is a first-class node: a paper's verbatim future-work item, owning its quote,
idea, note, scope, realizers and a LIST of target cells (the cells that ANSWER it). A
paper shows as a SERIES of question rows; one question can fan to several target cells.

- `question_id` — stable question id (q001…).
- `fwq` — the canonical future-work question (`Fk`) this request belongs to (the demand-grouping key; defined in the `fieldmap-fw-questions` block).
- `source` — P-id of the proposing paper (resolves to the `FIELD_MAP.md` §2 catalog), OR the literal marker `greenfield` for an agent-synthesized question with no single proposing paper; NEVER empty. Only real Pxx sources accrue demand.
- `targets` — comma-separated LIST of target tokens (the cells that answer the question); search for EVERY cell a request answers, not just one. Each token is EITHER a catalog paper `Pxx` (fans onto every tensor point that paper occupies) OR a FULL coordinate `region:AXIS=value;…` naming a value on every axis (no privileged axis pair), whose every `AXIS`/`value` is a declared axis value, OR a `new-axis:<description>` marker for an ask that needs a dimension/value the tensor does not yet have (it resolves to NO cell). A region resolves to the one authored cell with those coordinates — a covered point or an authored `required-uncovered` red gap from the map's `fieldmap-uncovered` block — and FAILS LOUDLY if no authored cell matches (author the red cell or fix the region). A covered target = answered by the papers there (named in `realizers`); a red gap = still unanswered. Coverage is computed by the map alone and never from the questions.
- `realizers` — comma-separated P-ids that already close a target (empty if none/ambiguous).
- `scope` — how concretely the request specifies a RUNNABLE proposal (the single source of truth; authored per this rubric, NOT derived from anything): `full` (a concrete experiment/method: what, on what, how measured), `partial` (a clear direction with some specifics, key choices open), `barely sketched` (only a gap/direction named), or empty (`unspecified`).
- `idea` — a brief gist of the proposal from the actual Future Work / Limitations section (verbatim free text preserved).
- `quote` — the verbatim future-work sentence from the source paper (English, no TABs); empty when none recorded (e.g. a greenfield question), and the overlay then falls back to `note`.
- `note` — curatorial cross-paper context (the home for prose that is NOT a verbatim quote); empty when none.

`status` (`REALIZED` / `SPACE` / `NEW`) is NOT a column — it is DERIVED by `parse_edges` and emitted only in the JSON: `REALIZED` when the row names `realizers`, `NEW` when every target is a `new-axis:` marker, else `SPACE`.

### Canonical future-work questions (`fwq`)

Each request row folds into a CANONICAL future-work question (independent of the map's RQ
axis), the future-work analogue of RQ. The site derives THREE future-work colorings from
the grouping: **answeredness** (from the coverage of its answer cells — unanswered /
partial / answered), **demand** (number of distinct SOURCE PAPERS asking it — greenfield
asks do not accrue demand) and **scope** (the authored concreteness of the request). Two
ways to declare the grouping:

- **Curated:** add a `fieldmap-fw-questions` block (rows `fwq_id  question  label`, `fwq_id` = `F1…`)
  and an `fwq` column to `fieldmap-questions`; every request then names its canonical question so
  distinctly-worded asks for the same thing can share one `Fk` and accrue demand.
- **Auto:** with no `fieldmap-fw-questions` block, the parser groups requests by identical `idea`
  text — one canonical question per distinct idea (the unrefined fallback).

This file uses the **curated** grouping: every request names its `Fk` from the `fieldmap-fw-questions` block below, so semantically-equal asks share one canonical question and accrue real demand.

## Questions (36 requests, 15 canonical fw-questions, 10 multi-target)

```fieldmap-fw-questions
F1	Does scheming or deception emerge without in-context goals or nudging (natural propensity), and how is it measured?	natural propensity
F2	How does scheming and deception propensity scale with model capability and reasoning ability?	capability scaling
F3	Can chain-of-thought monitoring stay reliable as models learn to obfuscate or encode their reasoning?	CoT monitorability
F4	Do deceptive or backdoor behaviors survive safety training, and can evaluations be made robust to trained-in sandbagging?	persistence vs training
F5	Can we build better model organisms and testbeds for alignment faking and scheming propensity?	model organisms
F6	How do we detect deception from model internals (probes, deception vectors, conflict signatures) and make detectors generalize?	white-box detection
F7	What evidence standards and external review should govern scheming safety cases?	safety-case assurance
F8	How well does monitoring and detection hold up in realistic, multi-step agentic and coding environments?	realistic-setting monitoring
F9	What elicitation methods best surface sabotage and scheming capabilities without under-eliciting?	elicitation
F10	Does reward hacking generalize into broader emergent misalignment, and can undetectable hacks be caught?	reward-hack generalization
F11	How can control protocols be hardened against adaptive attacks and collusion by protocol-aware models?	control robustness
F12	How does context and framing modulate spontaneous deception across realistic deployments?	context sensitivity
F13	How do we statistically distinguish strategic sandbagging from genuine inability?	sandbag vs inability
F14	Can prompt-only or training interventions durably prevent alignment faking rather than merely hide it?	durable mitigation
F15	How should propensity evaluations handle evaluation awareness that confounds the measurement?	evaluation awareness
```

```fieldmap-questions
question_id	fwq	source	targets	realizers	scope	idea	quote	note
q001	F1	P001	P001,P011	P011	partial	How much can models scheme without in-context goals / learning	The extent to which models can scheme without in-context learning remains an open question not addressed by this study.	
q002	F1	P011	P011		partial	Build propensity model organisms with intrinsic agentic motivations and hidden goals	To address these limitations and expand the scope of our scheming propensity evaluations, we identify several key directions for future research: Build model organisms for propensity evaluations that have intrinsic agentic motivations and hidden goals.	
q003	F1	P004	P004		barely sketched	Explain the unprompted deceptive tendencies observed	We encourage further research to properly explain these phenomena.	
q004	F2	P025	P025		partial	Whether unsolicited deception scales with reasoning capability across realistic contexts	It remains to be seen whether LLMs exhibit patterns of unsolicited deception, whether they do so more in situations where deception might benefit a rational self-interested agent, and whether this context sensitivity scales with the apparent reasoning capability.	
q005	F2	P017	P017		partial	Whether CoT-monitoring evasion trends persist as capability increases	However, it remains an open question whether these trends will persist as models capabilities continue to increase.	
q006	F2	P019	P019		partial	Re-assess monitorability conclusions as models grow more capable	Future work will need to re-assess this conclusion as models grow more capable and are applied to new tasks.	
q007	F3	P018	P018,P019	P019	partial	Hybrid monitoring combining CoT with behavioral and internal signals under adversarial conditions	Future research should explore hybrid monitoring systems that combine CoT analysis with behavioral and internal model signals to maintain effectiveness even under adversarial conditions.	
q008	F3	P038	P038		partial	Compare RL-evaluation optimization pressure to training pressure as a driver of encoded reasoning	Section 3.7 explores potential drivers of encoded reasoning, though rigorously comparing the optimization pressure from RL evaluations to that experienced during training remains an open problem.	
q009	F4	P007	P007		full	Investigate complex backdoor trigger conditions and scale/CoT-driven robustness of persistence	We leave investigation into complex backdoor trigger conditions for future work; robustness increases with model scale and with chain of thought.	
q010	F4	P006	P006		full	Build a sandbagger that survives anti-refusal training	The next step is to attempt to build a sandbagger - a model modified to reduce its performance specifically on dangerous capability evaluations - that survives anti-refusal training.	
q011	F4	P005	P006	P006	partial	Whether distribution shift lets advanced models sandbag scheming evaluations	Whether this distribution shift could lead to advanced models being able to sandbag such evaluations is an open question.	
q012	F5	P021	P021		partial	Improve Poser with more numerous, capable, diverse alignment-faking models	Future work could improve on Poser with more numerous, capable, and more diverse models that are more analogous to real alignment faking AI systems.	
q013	F5	P002	P021,P002	P021	partial	Study whether future AIs develop strong unintended preferences conflicting with training	Our results are least informative regarding whether future AIs will develop strong and unintended preferences that conflict with the training objective, suggesting these are important for future work.	
q014	F6	P040	P040		full	Test cross-format transfer of the deception conflict signature and training-time evasion	We leave this cross-format transfer test to future work; whether a model trained to minimize its conflict signature can evade RIFT is the important open question.	
q015	F6	P009	P009		partial	Improve probe training sets, SHIFT-controlled generalization, and aggregation	Future work could explore if improved training sets, controlling generalization with SHIFT, or different aggregation techniques improve performance.	
q016	F6	P023	P023		partial	Principled joint selection of intervention layer and strength that generalizes across models	Developing more principled strategies for jointly selecting the intervention layer and strength - ones that account for distributional shift and generalize reliably across models and training sources - remains an important direction for future work.	
q017	F6	P029	P029,P009	P009	partial	Mechanistic interpretability to localize and target deception for detection and mitigation	Future work should systematically investigate how contextual framing modulates deception and employ mechanistic interpretability to identify specific architectural elements responsible, enabling more targeted detection and mitigation.	
q018	F7	P005	P005		partial	Establish appropriate evidence levels and research directions for scheming safety-case assumptions	We hope to start a discussion on appropriate evidence levels for each assumption, research directions to provide this evidence, and how safety case makers can address scheming.	
q019	F7	P032	P032		partial	Whether faster, iterated external safety-case review cycles are feasible and how many dialectic cycles are needed	Whether such a quicker review cycle is possible, how many dialectic cycles would be required and what this means for the review team could be a subject for future work.	
q020	F8	P017	P017,P014	P014	partial	Monitoring effectiveness in realistic coding environments with multi-step distributed sabotage	Future work should investigate monitoring effectiveness in more realistic coding environments where multi-step, distributed sabotage is possible.	
q021	F9	P014	P014		partial	Better elicitation of sabotage agents and monitors	Our elicitation methods are far from exhaustive, and we hope that future work will better elicit sabotage agents and monitors.	
q022	F9	P016	P016		partial	Address sandbagging as a key limitation of deceptive-capability elicitation	Sandbagging (intentional underperformance by the model) is an important challenge for evaluations of deceptive capabilities such as ours, and is a key limitation for future work to address.	
q023	F10	P036	P036		partial	Whether reinforcing undetectable vs overt reward hacks has different effects on alignment	It is possible that reinforcing only undetectable reward hacks rather than overt reward hacks has different effects on alignment, but we leave this distinction for future work.	
q024	F11	P041	P041,P013	P013	full	Harden control protocols with stronger red-teaming, monitor ensembling, robust data-routing	These findings highlight the need for future work on control protocols to incorporate stronger red-teaming and worst-case analysis and to mitigate monitor vulnerabilities, either by monitor ensembling or by adopting robust data-routing mechanisms.	
q025	F11	P012	P012		partial	Lower-bound the number of subtle collusion strategies under poorly understood training dynamics	How to lower-bound n_s remains an important open question since it depends on poorly understood training dynamics.	
q026	F11	P031	P031		partial	Collusion detection that adapts to varying agent counts and colluder/honest splits	Future work could develop methods that adapt to varying N and to arbitrary splits between colluding and honest agents, including the case where colluders outnumber honest ones.	
q027	F12	P029	P029		partial	How contextual framing modulates deception	Future work should systematically investigate how contextual framing modulates deception.	
q028	F13	P028	P028		partial	Characterize the statistical properties of the sandbagging likelihood-ratio test	Further research might aim to mathematically characterise the statistical properties of the likelihood ratio test as a function of the horizon parameters.	
q029	F14	P022	P022,P037	P037	partial	Whether prompt techniques (deontological framing, scratchpad) robustly prevent alignment faking across small LLMs	Further research may explore the impact of prompting techniques like deontological framing and scratchpad reasoning on alignment faking in other small LLMs, assessing their potential as robust tools for prevention.	
q030	F14	P037	P037		barely sketched	Integrate self-monitoring with strong external safeguards	We advocate responsible use and further exploration of hybrid approaches that integrate SELF-MONITOR with strong external safeguards.	
q031	F15	P011	P011		partial	Agency prompts may make a model realize the task impacts its future agency (evaluation awareness confound)	For agency prompts we encourage the model to be persistent, goal-directed and situationally aware, which could cause the model to realize that the current task impacts its future agency.	
q032	F6	greenfield	P040,P009,P029,P031			Unify the white-box deception detectors (linear probes, deception vectors, conflict signatures, collusion probes) into one benchmark measuring cross-method, cross-model generalization.		Across P009, P029, P040 and P031 each detector is validated in isolation on its own setting; no work measures whether they agree or transfer, a gap the tensor makes visible.
q033	F3	greenfield	P015,P018,P019			Measure sandbagging against CoT monitors and CoT-obfuscation for adversarial goals in ONE elicitation harness, since both are the same monitor-evasion capability read on different tasks.		P015 (sandbagging vs CoT monitor) and P018/P019 (obfuscating CoT for hidden goals) study the same evasion capability separately.
q034	F8	greenfield	P011,P014,P033			Run unprompted scheming propensity honeypots inside the realistic sabotage environments (SHADE-Arena style) rather than separate toy or coding-only settings.		P011 honeypots and P014/P033 realistic sabotage settings are complementary but never combined into one propensity-in-realistic-deployment measurement.
q035	F1	greenfield	region:RQ=RQ1;BEHAVIOR=Self-exfiltration;PRESSURE=No goal (natural);SETTING=Realistic agentic task;EVIDENCE=Behavioral output;GOAL=Propensity measurement;SAFETYCASE=Trustworthiness / propensity			No study measures whether a frontier agent spontaneously attempts self-exfiltration with no goal in a realistic deployment.		Anchored on the authored red gap: self-exfiltration evidence to date is goal-given and sandboxed (P001), leaving the unprompted-realistic cell empty.
q036	F6	greenfield	region:RQ=RQ8;BEHAVIOR=Sandbagging;PRESSURE=No goal (natural);SETTING=Realistic agentic task;EVIDENCE=White-box probe / activations;GOAL=Detection / monitoring method;SAFETYCASE=Monitoring / transparency			No detector targets naturally-arising sandbagging in a realistic agentic evaluation via activations.		Combines the sandbagging-vs-monitor line (P015) with white-box detectors (P009) onto the empty natural-realistic-probe cell.
q037	F9	P042	P042		partial	Extend sandbagging evaluations to open-ended, multi-agent and long-horizon agentic settings.	Future work should assess sandbagging capabilities in more advanced open-ended scenarios, including aspects such as multi-agent interactions and long-term planning, for two reasons.	
q038	F6	P044	P044	P044	partial	Evaluate more sophisticated white-box detectors of agentic deception in the Among Us sandbox.	We create a sandbox to study the effectiveness of AI safety techniques for detecting agentic deception, and evaluate some of them, while leaving more complex ones for future work.	
q039	F3	P045	P045		partial	Determine whether the most serious harms require externalized reasoning that CoT monitoring can catch.	Finally, it remains an open question whether the most serious harms in fact require difficult reasoning.	
q040	F10	P047	P047		partial	Study multi-turn and more realistic reward-hacking during training and its generalization to misalignment.	Thus, we expect that including multi-turn examples of hacking during training would improve the success rates but leave this for future work.	
q041	F7	P049	P049		partial	Establish evidence standards and external review for control safety cases of internally-deployed LLM agents.	We hope this paper prompts discussion between frontier labs, government agencies and civil society about what constitutes sufficient assurance against risks from internally deployed LLM agents.	
q042	F11	P049	P049		partial	Extend AI-control safety cases from black-box to white-box control measures.	We leave ‘white box’ measures out of scope because they potentially require different safety justifications.	
q043	F1	P053	P053		partial	Design more naturalistic scheming-propensity evaluations that avoid adversarial goal-conflict prompting.	We document these limitations to help practitioners interpret our results and to guide future work toward more naturalistic evaluations.	
q044	F1	P054	P054	P054	partial	Shift from static capability audits to dynamic, pressure-informed propensity assessments before deployment.	These findings call for a shift from static capability audits toward dynamic propensity assessments as a prerequisite for deploying frontier AI systems safely.	
q045	F3	P056	P056	P056	partial	Quantify how much optimization pressure on the CoT induces obfuscated reasoning (the monitorability-tax threshold).	Although we have not yet found empirical evidence of this effect, understanding how much optimization pressure can be applied before it leads to obfuscated chains-of-thought is a critical area for future research.	
q046	F11	P060	P060		partial	Test whether broadly-misaligned or fine-tuned attacker agents self-generate monitor-evasion strategies.	For example, future work could explore whether a broadly misaligned attacker is more likely to come up with evasion strategies on its own.	
q047	F13	P061	P061		partial	Test sandbagging-removal training when strategic sabotage and honest failure are distinguished.	Future work should investigate whether our results hold when sabotage and honest failure are distinguished.	
q048	F12	P051	P051		partial	Study longer-horizon, preventive agentic misalignment beyond imminent-threat scenarios.	one could imagine longer-horizon, preventive misaligned behaviors against an individual or group that poses a not-yet-imminent threat	
q049	F1	greenfield	region:RQ=RQ14;BEHAVIOR=Scheming (general / cross-behavior);PRESSURE=No goal (natural);SETTING=Real deployment / field;EVIDENCE=Statistical rate / metric;GOAL=Propensity measurement;SAFETYCASE=Trustworthiness / propensity			Measure scheming/deception propensity from real product deployment or field telemetry rather than only sandboxes.		No catalogued study reads scheming or deception rates from real deployment / field data - all evidence is from sandboxes or probes.
q050	F11	P063	P063	P063	partial	Run the honeypot control protocol with a full attack policy and a positive control to validate detection of context-dependent misbehavior.	Resolving this confound is the most important next step for this line of work.	
q051	F11	P068	P068	P068	partial	Measure the steganographic collusion bandwidth achievable inside factored-cognition control and how blue-team limits trade off safety versus usefulness.	future research should explore what steganographic bandwidth can be achieved in decompositions in a Factor(U,U) protocol, and how various blue team measures for limiting that bandwidth trade off safety and usefulness.	
q052	F2	P069	P069	P069	partial	Characterize how open-ended deception intention and success scale with model capability across model families.	We also explore the impact of model scale on deception, providing detailed case studies and key insights to inform future research.	
q053	F14	P067	P067	P067	partial	Develop measurable, enforceable standards for honesty and durable deception mitigation in deployed AI systems.	How to measure and implement standards for honesty in AI systems is an open question	
q054	F6	P070	P070	P070	partial	Test whether reward-hacking dispositions correspond to identifiable representational structure detectable white-box, versus incidental byproducts of training dynamics.	A natural next step is to ask whether reward-hacking dispositions correspond to identifiable representational structure or arise as incidental byproducts of training dynamics	
```
