# FIELD_MAP_edges.md — the field's future-work questions

> The field's OWN future-work graph as QUESTIONS, separate from the tensor in FIELD_MAP.md.
> The graph is tripartite: paper -> question -> cell. From this TSV one can rebuild ANY
> diagram (by paper, by canonical question (fwq), by target cell). Built into
> backend/content/edges/ai_control.json by scripts.field_map.parse_edges. English only
> (32 sourced, 5 greenfield). The future-work graph lives only
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

## Questions (37 requests, 15 canonical fw-questions, 14 multi-target)

```fieldmap-fw-questions
F1	Extend control evaluations from toy/single-file settings to realistic multi-step, multi-file agentic deployments.	Toy to realistic agentic transfer
F2	Develop stronger red-teaming and learned/adaptive attack selection for control evaluations.	Stronger red-team / attack selection
F3	Harden monitors against adaptive and monitor-targeted attacks (ensembling, robust routing, factoring).	Monitor hardening vs adaptive attacks
F4	Build white-box / internal-state and hybrid detection for actions behavioral or CoT monitors miss.	White-box and hybrid detection
F5	Model memoryful / history-dependent control protocols and their equilibria.	Memoryful protocols and equilibria
F6	Develop robust elicitation and bounds for collusion, including acausal collusion and channel capacity.	Collusion elicitation and bounds
F7	Make control protocols cost-aware and optimize the control tax as a central objective.	Cost-aware control / control tax
F8	Develop AI-assisted auditing that needs less human involvement and lower human reliability.	AI-assisted auditing
F9	Study how monitor information access, architecture and signal diversity affect sabotage detection.	Monitor information and diversity
F10	Preserve and strengthen CoT monitorability under capability scaling and training pressure.	Preserving CoT monitorability
F11	Build comprehensive benchmarks/environments for subversion strategy and control.	Control benchmarks and environments
F12	Extend control safety cases to internally-deployed frontier agents and toward superintelligence.	Control safety cases at scale
F13	Train models to produce legible/checkable outputs to aid trusted monitoring.	Training for legibility
F14	Broaden sabotage and research-sabotage evaluations and improve their metrics.	Broader sabotage evaluations
F15	Study evaluation awareness / deployment detection and its confound on control measurements.	Evaluation awareness confound
```

```fieldmap-questions
question_id	fwq	source	targets	realizers	scope	idea	quote	note
q001	F1	P001	P004,P026	P004,P026	partial	Demonstrate control evaluations and techniques in more complicated and realistic agentic situations.	We are excited about future work demonstrating these evaluations and techniques in more complicated and realistic situations.	The founding paper asks for the toy-to-agentic transfer that Ctrl-Z (P004) and the SHADE-Arena protocol evaluation (P026) begin to deliver.
q002	F5	P002	P003	P003	partial	Analyze memoryful, history-dependent control protocols and their equilibria.	We leave the analysis of memoryful protocols, in which actions can depend on the full history of past observations, to future work.	The adaptive macro-protocol of P003 is a first step toward history-dependent protocols.
q003	F2	P003	P019,P020	P019,P020	partial	Evaluate more red-team micro-strategies and macro-protocols robust to biased estimates.	While not exhaustive, our experiment provides a blueprint for future work evaluating more red team micro-strategies.	Attack-selection work (P019, P020) shows the red team's strategy choice sharply lowers measured safety.
q004	F8	P004	new-axis:AI-assisted auditing with lower human reliability		partial	Investigate AI-assisted auditing techniques that require less human involvement and lower reliability.	Future work needs to investigate AI-assisted auditing techniques that require less involvement and lower reliability from humans.	No catalogued protocol yet reduces the human-auditor reliability the control frontier assumes.
q005	F7	P004	new-axis:deployment cost as a central optimization target		partial	Take deployment cost into account as a central optimization target for control protocols.	We believe future work should consider taking cost into account as a more central optimization target.	The control tax is measured but not yet optimized as the primary objective.
q006	F11	P005	P021,P041	P021,P041	partial	Expand the subversion-strategy methodology into a comprehensive benchmark.	We hope for a future work to expand on this methodology to create a comprehensive benchmark.	SLEIGHT-Bench (P021) and MonitorBench (P041) extend the evaluation surface for subversion and monitoring.
q007	F4	P006	P043,P044	P043,P044	partial	Develop white-box methods that detect scheming via invariant activation patterns.	Whitebox methods remain a promising direction for future research: they could offer more direct and reliable ways to detect scheming if we can identify invariant patterns in model activations.	Probe work (P043 negative results, P044 off-policy generalization) directly tests this white-box direction.
q008	F12	P007	P036,P006	P036	partial	Build a comprehensive control safety case for internally deployed frontier LLM agents.	We hope this paper prompts discussion about what constitutes sufficient assurance against risks from internally deployed LLM agents.	The CoT-monitoring roadmap (P036) and the trajectory framework (P006) develop the safety-case argument.
q009	F6	P007	P014,P022	P014,P022	partial	Quantify how communication-channel capacity translates into models' ability to coordinate attacks.	It remains an open question how the capacity of a given communication channel translates into the ability of models to coordinate attacks.	Collusion studies (P022 focal points, P014 untrusted-monitoring safety case) address the coordination bound.
q010	F3	P008	P012,P015	P012,P015	full	Incorporate stronger red-teaming and mitigate monitor vulnerabilities via ensembling or robust data routing.	These findings highlight the need for future work on control protocols to incorporate stronger red-teaming and to mitigate monitor vulnerabilities, either by monitor ensembling or by adopting robust data-routing mechanisms.	Ensemble monitoring (P012) and TraceGuard (P015) are the ensembling / robust-routing responses.
q011	F13	P009	P009	P009	partial	Train models to write outputs legibly rather than only prompting for legibility.	Another area of future work is to train language models to write outputs legibly, instead of just prompting.	Legibility is currently obtained by prompting; training for it is open.
q012	F3	P010	new-axis:steganographic bandwidth in factored decompositions		partial	Explore steganographic bandwidth in factored decompositions and blue-team measures to limit it.	Future research should explore what steganographic bandwidth can be achieved in decompositions in a Factor(U,U) protocol, and how blue-team measures for limiting that bandwidth trade off safety and usefulness.	No catalogued paper measures the steganographic channel a factored protocol opens.
q013	F3	P011	P011	P011	partial	Test whether giving plan-monitors partial implementation context improves safety.	Future research should investigate whether changes to Factor(U,T), such as allowing monitors partial implementation context, can improve safety.	Factor(U,T) (P011) opens the plan-monitoring direction it asks to extend.
q014	F9	P012	P016	P016	partial	Build monitors from architecturally diverse sources to test whether diversity yields robustness beyond prompt/fine-tune variation.	Future work is needed to build monitors from different sources to test whether architectural diversity yields robustness gains beyond what fine-tuning and prompting variation provide.	The information-access study (P016) probes what makes a monitor detect sabotage.
q015	F1	P012	P004,P025	P004,P025	partial	Test monitor generalization to realistic deployments with ambiguous intent, longer horizons and distributed attacks.	How well these findings generalize to realistic deployments with ambiguous attacker intent, longer agent horizons, or attacks distributed across multiple episodes remains an open question.	SHADE-Arena (P025) and Ctrl-Z (P004) supply longer-horizon realistic settings.
q016	F6	P014	new-axis:robust elicitation for acausal collusion		partial	Develop robust elicitation methods for acausal collusion.	Developing robust elicitation methods for acausal collusion remains an open problem.	Untrusted monitoring's trust rests on collusion elicitation that no catalogued method provides.
q017	F9	P015	P016	P016	partial	Evaluate whether monitoring dimensions generalize across ControlArena settings (infrastructure-as-code, APPS, vLLM).	Evaluating whether the same five monitoring dimensions generalize or require domain-specific adaptation across ControlArena settings is an important next step.	Information-access work (P016) studies monitor generalization across settings.
q018	F9	P016	P016		partial	Study how attacker trajectory length, serial-reasoning depth and main/side-task similarity affect monitor performance.	Future work should study how attacker trajectory length, the amount of serial reasoning required for the side task, and the similarity of main and side tasks affect monitor performance.	An open direction the paper names but does not close.
q019	F2	P019	new-axis:learned attack selectors and adaptive blue-team responses		full	Develop learned (not prompted) attack selectors and blue-team protocols that adapt to selection.	Several directions remain for future work: learned (rather than prompted) selectors, blue-team protocols that adapt to selection, and deployment-level metrics that account for how the attack rate shifts under selection.	No catalogued protocol yet adapts to a learned attack selector.
q020	F11	P021	new-axis:catalog of monitor blind spots		partial	Continue enumerating monitor blind spots that uplift a monitoring red-teamer.	One important direction for future work is to continue enumerating monitor blind spots: conceptual insights that significantly uplift a monitoring red-teamer.	A running catalog of monitor blind spots does not yet exist as a shared artifact.
q021	F14	P024	P029,P027	P029,P027	partial	Build more sophisticated sabotage evaluations, including a sandbagger that survives anti-refusal training.	We hope that the protocols and materials provided in this paper will serve as a starting point for future, more sophisticated evaluations of related capabilities.	CTRL-ALT-DECEIT (P029) and Auditing Sabotage Bench (P027) extend sabotage evaluation.
q022	F14	P025	P025		partial	Better elicit sabotage agents and monitors and use the suite as a pre-deployment dangerous-capability evaluation.	Our elicitation methods are far from exhaustive, and we hope that future work will better elicit sabotage agents and monitors.	SHADE-Arena's own elicitation is acknowledged as non-exhaustive.
q023	F1	P026	new-axis:protocol variants raising usefulness at high safety		partial	Explore protocol variants that increase usefulness while maintaining very high safety robustness.	We hope to see further variants of this strategy explored, in particular variants that increase usefulness while maintaining very high robustness on safety.	The high-safety/high-usefulness corner of the frontier is not yet occupied.
q024	F10	P032	P039	P039	partial	Investigate whether CoT-monitoring effectiveness persists as model capability increases.	It remains an open question whether these CoT-monitoring trends will persist as model capabilities continue to increase.	Reasoning-Under-Pressure (P039) shows training pressure already degrades monitorability.
q025	F1	P032	P025	P025	partial	Investigate CoT-monitoring in realistic coding environments with multi-step, distributed sabotage.	Future work should investigate monitoring effectiveness in more realistic coding environments where multi-step, distributed sabotage is possible.	SHADE-Arena (P025) supplies the realistic multi-step sabotage setting.
q026	F15	P032	new-axis:detect and mitigate evaluation awareness in monitoring		partial	Detect or mitigate evaluation awareness so CoT monitoring reflects genuine reasoning.	Future research should explore ways to detect or mitigate evaluation awareness, such as randomized test framing, so CoT monitoring reflects genuine reasoning rather than performative alignment.	Evaluation awareness confounds every control measurement but has no dedicated mitigation here.
q027	F4	P034	P043		partial	Build hybrid monitoring combining CoT analysis with behavioral and internal signals.	Future research should explore hybrid monitoring systems that combine CoT analysis with behavioral and internal model signals to maintain effectiveness under adversarial conditions.	Probe negative results (P043) motivate fusing signals rather than trusting internals alone.
q028	F12	P033	P036	P036	partial	Turn 'models cannot achieve top-human stealth without being caught by a CoT monitor' into a safety case.	A safety case could argue that models cannot perform top-human-level stealth without being caught by a CoT monitor.	The CoT-monitoring roadmap (P036) develops exactly this safety-case shape.
q029	F10	P035	P041,P037	P041,P037	partial	Invest in research on CoT monitorability alongside existing safety methods.	We recommend further research into CoT monitorability and investment in CoT monitoring alongside existing safety methods.	MonitorBench (P041) and the faithfulness+verbosity measure (P037) operationalize monitorability.
q030	F14	P030	P031	P031	partial	Broaden alignment testing beyond safety-research sabotage.	We plan to broaden the scope of our alignment testing to cover areas aside from safety-research sabotage in the future.	Gram (P031) generalizes sabotage-propensity auditing across tasks.
q031	F14	P031	new-axis:sabotage-propensity metrics not relying on chain-of-thought		partial	Develop sabotage-propensity metrics that do not rely on the chain-of-thought.	Future work should develop improved evaluation metrics not relying on chain-of-thought.	All current propensity readouts here lean on the CoT, which may itself be gamed.
q032	F2	P011	P019	P019	partial	Evaluate additional attack strategies to test generalizability across attack classes.	Future work should evaluate additional attack strategies to assess the generalizability of our findings across attack classes.	Attack-selection work (P019) systematically varies the attack policy.
q033	F4	greenfield	P043,P044		partial	Can a fused behavioral + CoT + white-box monitor beat an adaptive attacker where each single channel fails?		Across P006, P008 and P034 each single monitoring channel is shown evadable and P043 finds probes read the situation not the action; no catalogued paper builds and validates the fused monitor against an adaptive attacker.
q034	F6	greenfield	P022,P023		partial	What is the minimal shared context or channel under which untrusted monitors reliably collude, across single- and multi-agent settings?		Synthesizes the focal-point collusion of P022 and the multi-agent SCHEME result of P023 into a channel-capacity question neither measures directly.
q035	F1	greenfield	P004,P026		partial	Does the protocol ranking (resample over defer over monitor) seen in toy/APPS settings hold in realistic multi-file agentic deployments, or invert?		The head-to-head transfer implied by comparing P001/P002 (toy) with P004/P026 (agentic) is not measured directly anywhere in the catalog.
q036	F10	greenfield	P039,P042		barely sketched	Do multilingual and training-pressure CoT degradation compound, so a capable model under RL pressure reasoning in a low-resource language becomes effectively unmonitorable?		Combines the language-fragility failure (P042) and the training-pressure failure (P039) into a compounded worst case no paper has tested.
q037	F15	greenfield	region:RQ=RQ15;PROTOCOL=Trusted monitoring;THREAT=Covert side-objective;SETTING=Structured agentic sandbox;MONITOR=Trusted action monitor;GOAL=Measurement & modeling		partial	Does specifying the forbidden behaviour abstractly ('do not subvert oversight') versus as concrete enumerated actions change the monitor's catch rate at equal usefulness?		Red-gap-anchored: RQ15 (how to specify what the untrusted model must not do) is entirely uncovered in the catalog; this poses the experiment that would fill the authored specification gap.
```
