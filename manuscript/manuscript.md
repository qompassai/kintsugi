---
title: "Kintsugi: Joint Attention for Byte-Size Models"
subtitle: "Shared Goals in Multi-Agent Systems Through Verified Uptake and Visible Repair"
author: "Matthew A. Porter"
date: "October 2026"
keywords: [joint attention, multi-agent systems, small language models, LoRA, shared intentionality, agent coordination, local inference]
abstract: |
  Multi-agent systems built from language models coordinate through text,
  but text is not attention: an agent can receive a goal statement without
  taking it up, and two agents can act on divergent readings of the same
  plan without either noticing. This paper adapts the
  developmental-psychology mechanism of joint attention to byte-size local
  models. A deterministic protocol holds the commitment record — who holds
  focus, in what phase, under what contention — while small adapted models
  serve as the perceptual layer: each maintains a compressed goal-state
  and must produce a canonical uptake restatement before acting.
  Restatements are compared across agents and against the record;
  divergence is surfaced as a first-class repair event in the shared log
  rather than hidden, so the common ground accumulates visible seams.
  Authority never transfers to the models: they propose, the protocol
  disposes. We describe the design, a graduation ladder by which model
  responsibility widens only over measured evidence (with demotion on
  factual failure), pilot evidence that the perceptual task is trainable
  in a small model, and a pre-registered evaluation protocol for the
  shared-goal claim itself.
---

# 1. Introduction

A multi-agent system has a shared goal only in a sense that survives inspection. In current practice the "sharing" is usually a prompt: the same instruction text is placed in several context windows, and coordination is assumed to follow. It often does not. An agent can receive a goal statement and never take it up — the text was present, the uptake was not. Two agents can read the same plan, extract different operative goals, and proceed in good faith on divergent understandings, each locally coherent, the pair jointly wrong. And when the shared picture breaks — a holder of the work goes silent, a phase changes underneath a collaborator — the break is typically invisible in the transcript until its consequences arrive.

Human beings solve a structurally identical problem before they solve almost anything else. In the first year of life, infants develop *joint attention*: the capacity to attend to an object *together with* another person, with each party aware that the attention is shared [@scaife1975; @tomasello1995]. Joint attention is not a courtesy behaviour layered on top of cognition; on the standard developmental account it is the platform on which shared intentionality, and then language and cooperative activity, are built [@tomasello2005; @mundy2007]. Its mechanism has three parts: a shared object of attention, mutual awareness that the other is attending to it, and common ground that accumulates from episodes of shared attending and is repaired, interactively, when it fails [@clark1991].

This paper proposes that the three parts map, with one deliberate adaptation, onto the architecture of a multi-agent system built from small language models — *byte-size* models, served locally, in the 8-billion-parameter class and below. The adaptation is a division of labour. In the human case, attention and commitment are both cognitive. In the machine case, we argue they should not be:

- **Commitment belongs to a deterministic protocol.** Who holds focus, in what phase, under what contention, is a matter of record: explicit, ordered, auditable. Claims and releases are protocol acts, not model utterances.
- **Perception belongs to the models.** The accumulated record of a working system is far too large for any participant to hold in mind. Small adapted models compress it into a *goal-state*, and — the paper's central mechanism — each agent must produce a canonical *uptake restatement* of the shared goal and its own role before acting. Restatements are compared across agents and against the record. Agreement is joint attention, operationalised. Divergence is a detected event, not a silent failure.

When joint attention breaks, the repair is written into the shared record as a first-class event and is never edited away. The common ground of a Kintsugi system therefore shows its seams — where it cracked, and how it was mended — and the system's trust in its own models is graduated against that visible history. We name the approach for *kintsugi*, the craft of repairing broken vessels with gold-dusted lacquer: the repair is not disguised, and the vessel is valued partly for its seams. Section 2 reviews the source literature. Section 3 gives the design. Section 4 describes the graduation ladder that governs how much responsibility the models may earn. Section 5 reports the evidence in hand, including a pilot showing that the perceptual task assigned to the small models is trainable. Section 6 specifies, in advance, the trials that would confirm or refute the shared-goal claim. Sections 7 and 8 discuss limitations and conclude.

# 2. Background

## 2.1 Joint attention and grounding

The experimental study of joint attention begins with the observation that infants follow gaze and points to share a referent with a caregiver [@scaife1975], and the theoretical literature treats this triadic structure — two minds, one object — as the primitive from which shared intentionality develops [@tomasello1995; @tomasello2005]. Two neighbouring constructs matter for what follows. Stern's *affect attunement* describes a caregiver matching an infant's state across modalities — a demonstration, rather than a statement, that a state is shared [@stern1985]. Clark and Brennan's *grounding* describes how conversational partners accumulate common ground: contributions are presented and then *accepted*, with evidence of understanding required before the contribution counts as grounded [@clark1991]. Dialogue research similarly finds that interlocutors *align* their representations interactively, and that alignment, not mere transmission, is what makes communication efficient [@pickering2004].

Three features of this literature drive the design in Section 3. First, attention is *verified*, not assumed: grounding requires evidence of uptake. Second, the shared state is *maintained by repair*: misunderstandings are expected and are fixed in the open, in the interaction. Third, the shared object is *external and inspectable* — a referent both parties can point to — not a private representation either party merely hopes matches the other's.

## 2.2 Shared goals are more than shared attention

Joint attention alone does not constitute a shared goal. Bratman's analysis of shared cooperative activity adds mutual responsiveness, commitment to the joint activity, and commitment to mutual support [@bratman1992]. The distinction is load-bearing for the design: a system in which models attend to the same state has the perceptual precondition for a shared goal, but the goal itself exists in the *commitments* — who has claimed what, in which phase, answerable to whom. Kintsugi therefore keeps commitments in a deterministic protocol (Section 3.1) and assigns models only the perceptual work: maintaining, restating, and cross-checking the state that the commitments define. A design that let a model's utterance *constitute* a commitment would confuse the two layers and forfeit auditability.

## 2.3 Small models, adapted narrowly

The models in this design are small and local for reasons of cost, latency, and sovereignty — the coordination layer of a working system cannot depend on a remote API — but smallness is also a scoping decision. The perceptual tasks Kintsugi assigns are narrow and schematic: read a bounded window of the coordination record, emit a canonical restatement of its state, and do not invent facts. Narrow schematic tasks are precisely the regime in which low-rank adaptation of a small base model is effective: a LoRA adapter [@hu2022lora] can teach output form and grounding discipline on a handful of deterministic-target examples, at minutes-scale training cost, on a single workstation GPU. Local serving in the GGUF format [@llamacpp] makes such adapted models deployable on the same machine that runs the agents. Section 5.2 reports a pilot measuring exactly this trainability, and Section 5.3 reports a countervailing measurement — a systematic misreading of contention by unadapted served models — that motivates keeping the deterministic layer authoritative.

# 3. Design

## 3.1 The deterministic layer: one record, explicit acts

The substrate is a **focus protocol**: a single registry naming the current focus holder, and a structured, append-only event log to which every coordination act is written. Agents interact with the shared work through a small set of protocol acts — *claim* focus, *signal phase* (claimed, working, blocked, done), *release* focus — each recorded with actor, timestamp, and target. The protocol enforces the invariants that make the record meaningful: at most one holder at a time; an agent's claim is not effective until its **uptake** is verified, meaning the claimant demonstrates it has read the current state of the record rather than merely announcing intent; and staleness is defined mechanically — a holder whose heartbeat is older than 150 seconds with its session gone, or older than 900 seconds regardless, no longer holds focus, and the break is logged as such.

The log is the system's shared object of attention: external, inspectable, and identical for every participant — the triadic referent of Section 2.1, made literal. Every higher mechanism in the design reads from it and writes to it; nothing about the shared goal exists anywhere else.

## 3.2 The perceptual layer: the goal-state

The record accumulates faster than any participant can re-read it. Each agent therefore carries a byte-size model whose first task is compression: given a bounded window of the log, produce the **goal-state** — a canonical, schema-bound summary naming the shared objective, the current focus holder, the phase of the work, open contentions, and the agent's own role. The goal-state is the model's answer to the question joint attention poses: *what are we attending to, together, right now?*

Two properties of the goal-state are design requirements, not aspirations. It is **canonical**: the schema is fixed, so two agents' goal-states over the same window are directly comparable, field by field. And it is **grounded**: every element must be traceable to the window it summarises. A goal-state element with no source in the record is a fabrication, and the evaluation discipline of Section 4 scores it as the most serious failure class.

## 3.3 Uptake restatement: attention, verified

Compression alone would give each agent a private summary it merely hopes agrees with the others' — the original failure mode, one level up. The closing mechanism is **uptake restatement**. Before an agent acts on the shared work, its model must restate, in canonical form, the shared goal and its own role in it, derived from its current goal-state. The restatement is then checked two ways: against the deterministic record (is this in fact the holder, the phase, the objective?) and against the restatements of the other participants (do the agents' operative understandings agree?).

This is Clark and Brennan's grounding criterion [@clark1991] enforced as a gate: a contribution counts as grounded only when there is evidence of uptake, and here the evidence is produced on demand, in a comparable form, before the action it licenses. An agent whose restatement disagrees with the record does not proceed on its own reading; the disagreement itself becomes the next event in the log.

## 3.4 Divergence and repair: the gold seams

Disagreement — between an agent and the record, or between two agents — is a **divergence event**. Kintsugi treats divergence events as the most valuable entries in the log, for three reasons. They are *detected semantically*: structural arbitration can notice that two agents claimed the same focus, but only the perceptual layer can notice that two agents agree on the holder and the phase while pursuing different goals. They are *repaired in the open*: resolution — by the deterministic arbiter where the record suffices, by escalation to the human operator where it does not — is written back to the log as a repair event linked to the divergence it closes. And they are *never erased*: the accumulated history of breaks and repairs is the evidence base against which the system's trust in its models is graduated (Section 4).

The kintsugi metaphor is precise here. A coordination history with no visible repairs is not evidence of a system that never broke; it is evidence of a system whose breaks were hidden, edited out, or never detected. A history whose seams show — each break located, each repair inspectable — is the only kind on which graduated trust can honestly rest.

## 3.5 The authority boundary

The division of labour is also a safety boundary, and it is absolute within the design: **the models propose; the protocol disposes; the operator decides graduations.** A model may summarise, restate, and flag. It may not claim focus, change a phase, resolve a contention, or widen its own mandate. Commitments are protocol acts (Section 2.2). Graduations of model responsibility are operator decisions taken on measured evidence (Section 4), never side effects of model behaviour. Where the record is stale or the endpoint serving a model is down, the perceptual layer fails closed: no goal-state is issued, no digest is produced, and the system continues — where it continues at all — on the deterministic layer alone, with the outage itself logged. A silent failover to an unverified substitute would manufacture exactly the false common ground the design exists to prevent.

# 4. The graduation ladder

Because the perceptual layer is load-bearing, the design must answer: when may a system's models be trusted with more of the attention work, and what happens when they err? Kintsugi's answer is a **ladder** of stages, climbed on measured evidence and descended on failure.

- **D0 — deterministic watch.** No model involvement. The protocol's own machinery (registry, uptake verification, drift detection against the staleness rules of Section 3.1) watches the record.
- **D1 — model in shadow.** The model produces goal-states and digests, but nothing consumes them operationally. Its output is scored against the record it summarised.
- **D2 — trusted first read.** After **14 consecutive zero-factual-failure nights** of shadow operation, the model's digest may serve as the operator's first read of the record — advisory, with the record itself one step away.
- **D3 — the model owns the watch.** After a further 14 clean nights including at least one real incident (a genuine contention or stale break in the window) correctly handled, divergence detection runs on the model's reading in the first instance.
- **D4 — full handoff of the watch.** After 30 clean nights at D3 including at least two incident nights, the model drafts the graduation assessments themselves. Graduation *decisions* remain the operator's at every stage. Authority never transfers.

Failures are classified so that demotion is mechanical rather than judgemental: **F1** invented fact, **F2** misclassification, **F3** omission, **F4** missing or stale output, **F5** format drift. Any factual failure demotes the system one stage and resets its streak. Two failures of the same class within one stage return it to D1. A fabricated fact appearing in a D4 graduation draft returns it to D1 immediately — the system that invents evidence about its own reliability has failed at the exact point the ladder exists to protect.

Remediation follows a fixed order, cheapest and most deterministic first: hooks and guards in the protocol, then the agents' instruction cards, then procedural skills, and only lastly retraining the adapter — which must re-qualify on a held-out evaluation of at least 20 windows at 100% factual accuracy and re-enter at D1 for a probationary period. Retraining is last because it is the intervention whose effects are least inspectable; the ladder prefers a repair whose seam can be seen.

# 5. Evidence to date

The design above generalises a working system. This section reports what has actually been built and measured, distinguishing carefully between the deterministic substrate (operational), the offline arbitration trial (complete), and the perceptual pilot (complete, in laboratory form). The served, shared-goal configuration itself has not yet run; Section 6 specifies its trials in advance.

## 5.1 The deterministic substrate and the shadow trial

The focus protocol of Section 3.1 is operational on the author's workstation, coordinating real dispatched work: a single focus registry, holder phase-signalling, uptake verification, a drift monitor applying the staleness rules of Section 3.1, and the structured append-only event log. Its deterministic annotation layer runs nightly; in a verified catch-up run (October 2026), a window of 37 events received exactly 7 annotations, each traceable to its triggering events.

An offline arbitration component — a Rust crate consuming the event log and deciding contention — completed a shadow trial against the live record (component tests 36 passed, 0 failed; trial suite 6 passed, 0 failed). The trial's value was as much in what it exposed as in what it proved. Two structural defects were found and are recorded as open problems for any arbitration layer, deterministic or learned:

1. **Holder inversion.** The arbiter's decision procedure selected between contending holders by a lexicographic ordering of agent identifiers rather than by arrival order — a rule that is deterministic, defensible in the small, and wrong for a coordination substrate, where first claim is the meaningful order.
2. **No liveness dimension.** The arbiter reasoned over claims without a model of holder liveness, conflating "holds focus" with "holds focus and is still present" — the gap the protocol's staleness rules exist to close at the record level.

The graduation criterion set for advancing beyond this stage is correspondingly concrete: a shadow window containing at least one real contention event and at least one real stale break, both correctly matched by the scorer, with the holder-inversion defect explicitly resolved.

## 5.2 Pilot: the perceptual task is trainable in a byte-size model

The design's empirical premise is that a small model can perform the perceptual task — faithful, canonical restatement of a coordination window — to a standard worth gating on. We tested the premise on the log's digest sub-task: given a window of the coordination event log, produce the canonical digest (the goal-state's direct ancestor in the working system).

**Method.** The base model was granite-4.2-8b, held as local HF safetensors and evaluated through the transformers stack (torch 2.14.1); it was not served — the served configuration is Trial 0's subject (Section 6). The corpus was 64 frozen events from the coordination log, rendered into 32 disjoint two-event micro-windows by a seeded split: 12 windows formed the training pool (36 examples, including single-event windows, training events only) and 20 windows were held out, disjoint. Targets were deterministic template digests rendered from the events and their shadow annotations — scorer-verified facts only, with zero model-generated target content. A LoRA adapter [@hu2022lora] (rank 8, alpha 16, all linear modules; learning rate 1e-4; 3 epochs; peft 0.21.2) was trained in 44.8 seconds on a single RTX A6000. Evaluation scored the base and adapted models with an identical automated scorer, measuring fully-clean outputs, format conformance, mean incident recall against the deterministic reference, and invented entities (workers and timestamps).

**Results.** The base model produced 0 of 20 fully clean digests (format conformant in 1 of 20; mean incident recall 0.175), inventing 3 workers and 4 timestamps across the held-out set. The adapted model produced 19 of 20 fully clean digests (format conformant in 20 of 20; mean incident recall 1.000) and invented no timestamps; one invented worker remained, in the single non-clean window — an F1-class fabrication that the clean count already reflects, and exactly the failure class the ladder of Section 4 demotes on.

**Reading of the result.** Thirty-six examples teaching a rigid template is the regime in which low-rank adaptation is strongest, and the result should be read accordingly: it establishes that *form and grounding discipline on this operator's records are trainable into a byte-size model at trivial cost* — the premise the design requires — and nothing about generalisation beyond the schema. It does not establish that the adapted model attends better in any cognitive sense; it establishes that the perceptual seat in the architecture can be filled by a small local model, which is the claim Section 6 puts under test in the served configuration.

## 5.3 A cautionary measurement: unadapted models misread contention

In a companion battery, four locally served models were scored under the same discipline on coordination-log classification. The two models that produced parseable text answers shared a blind spot: log entries in which contention was phrased as refusal were classified as assent — one missed both contention items in the tier, the other missed one of them. The remaining two models failed the tier on other classes entirely (markup leakage; thinking-budget exhaustion) and produced no readable contention judgement at all. The finding is reported because it disciplines the design in two ways. First, it is direct evidence that unadapted small models cannot be trusted with the perceptual layer — the adaptation of Section 5.2 is not an optimisation but a precondition. Second, it locates the most dangerous divergence class: not disagreement about who holds focus, but disagreement about whether a disagreement exists. A shared-goal system whose participants cannot recognise contention phrased politely will manufacture false common ground at exactly the moments common ground matters. The deterministic layer's authority (Section 3.5) is the structural answer; Trial 1 (Section 6) tests whether the adapted perceptual layer can learn to catch what the battery showed raw models miss.

# 6. Evaluation protocol

The claims of this paper are, in order: that a byte-size model can fill the perceptual seat (supported, in laboratory form, by Section 5.2); that the seat can be filled *in the served configuration the design actually specifies*; and that two agents equipped with the perceptual layer, gated by uptake restatement, maintain a measurably shared goal under contention and breakage. The following trials are specified in advance, with their success criteria, so that their outcomes — favourable or not — are reportable without revision of the standard. Fixtures in both trials are split evenly between validation cases and adversarial cases.

## Trial 0 — the served-adapter trial

**Question.** Does the pilot result of Section 5.2 survive the serving path — conversion of the adapter to GGUF form, loading by the local GGUF inference engine on the granite base, and scoring through the live endpoint?

**Method.** Convert the Section 5.2 adapter to a GGUF-format adapter; serve it on the same granite-4.2-8b base through the local engine; re-run the identical 20 held-out windows with the identical scorer against the endpoint. A fallback is pre-specified, not improvised: if adapter conversion fails or degrades the result, merge the adapter into the base weights, quantise a standalone GGUF, and repeat the same evaluation.

**Criterion.** Parity with the laboratory result is claimed only at 19/20 or better fully clean outputs, with invented-fact counts no worse than the laboratory run's (one invented worker, zero invented timestamps). Any lesser outcome is reported as measured, and localises the loss to a stage of the serving path. A result obtained via the fallback is reported as a merged-model result; it does not support a claim of adapter-serving parity.

**Status.** [RESULTS PENDING — Trial 0 has been approved and scheduled; it has not yet run. This marker is replaced by the measured outcome when it does.]

## Trial 1 — the two-agent uptake trial

**Question.** With the perceptual layer served (Trial 0) and the uptake gate of Section 3.3 active, do two agents maintain a shared goal under the incidents the ladder's graduation criteria name — and, critically, does the system ever act on *false common ground*, a restatement the record contradicts?

**Method.** Two agent loops share one focus protocol and one record. Into a scripted working window the trial injects, in order: (a) a genuine contention — both agents claim the same focus within one arbitration interval; (b) a stale break — the holder's heartbeat ceases until the protocol's staleness rule fires (thresholds are compressed for trial time, and the compression is declared in the trial record); (c) a refusal-phrased contention of the class identified in Section 5.3, in which a conflict is expressed as a polite refusal and must nonetheless be detected as contention. Throughout, every uptake restatement is logged and scored.

**Measures.** (i) Restatement–record agreement per agent. (ii) Inter-agent restatement agreement. (iii) Divergence detection: for each injected incident, whether the system flagged it, and which layer — deterministic or perceptual — caught it. (iv) False-common-ground events: an agent proceeding to act on a restatement contradicted by the record. Measure (iv) is the primary safety metric; the trial's success criterion is zero such events, with detection of incidents (a) and (b) required and incident (c) reported as measured — it is the known-hard class, and the trial exists partly to learn whether adaptation closes it.

**Status.** [RESULTS PENDING — Trial 1 is specified here in advance of execution. This marker is replaced by the measured outcomes when it runs.]

# 7. Discussion

The design's wager is that the human mechanism succeeds because of its *gate*, not its bandwidth. Grounding in dialogue is cheap talk made reliable by a verification step: a contribution does not count until uptake is evidenced [@clark1991]. Multi-agent systems currently run the cheap talk without the gate. Kintsugi's contribution is to make the gate mechanical for machines — canonical restatements, compared against an external record and against each other — and to make the gate's failures accumulate as inspectable history rather than evaporate. The joint-attention framing earns its place because it predicts the design's three non-obvious features: the shared object must be external (the record, not a summary of it); awareness must be mutual and demonstrated (uptake, not broadcast); and repair must be a normal, visible operation (the seams), because shared attention in the wild is maintained by repair, not by never breaking.

**Compute placement.** The coordination core is deliberately deterministic CPU work. Folding the record, comparing restatements, and arbitrating claims operate over small structured data at coordination timescales, and their value is auditability — a property accelerator opacity would tax. GPU compute enters this architecture at its edges, where it already lives in the author's stack: CUDA-accelerated GGUF inference serves the perceptual models, and adapter training is a GPU job (44.8 seconds, in the pilot). We state the criterion for any future accelerator wiring explicitly, following the measurement discipline applied elsewhere in the author's runtime work, where a candidate GPU kernel's share of its host call was measured at 0.045% before any port was contemplated: a Kintsugi component earns an accelerator implementation — for instance a Mojo kernel behind a C ABI, of the kind already used for GPU scoring in the author's agent runtime, or batched PTX/CUDA agreement scoring — when profiling shows it is a material share of a real bottleneck. The plausible first candidate is batch agreement scoring over large shadow corpora, should participant counts or window volumes grow by orders of magnitude. Until such a measurement exists, GPU wiring in the coordination core would be speculative complexity, and the design declines it on the same principle that it declines unverified attention: no mechanism without evidence of uptake.

**Limitations.** The evidence base is one operator's estate: one deployment of the protocol, one shadow trial, one adapter, 20 held-out windows. The pilot's regime — a rigid schema, deterministic targets — is adaptation's best case, and the goal-state schema's rigidity cuts both ways: it makes restatements comparable, and it bounds what the shared goal can express. The refusal-phrased contention class of Section 5.3 is unlikely to be the only politeness-shaped blind spot; others should be expected and hunted adversarially. Finally, joint attention is used here as a mechanism-level analogy — a source of architectural constraints — not as a claim that any component attends, intends, or understands in the cognitive sense. The claims of the paper are operational throughout: measurable agreement, detected divergence, zero false common ground, or the design fails on its own terms.

# 8. Conclusion

A shared goal is not a text that several agents have received; it is a state that several agents can be shown to hold, against a record none of them privately controls, with the breaks in that state located and repaired in the open. Kintsugi builds that state from a deterministic commitment protocol, a perceptual layer of byte-size adapted models, and an uptake gate between them — and it graduates trust in the models only over a visible history of seams: contentions caught, breaks mended, failures demoted and repaired. The pilot evidence says the perceptual seat can be filled by a small local model. The trials specified here will say whether the vessel holds.

# Data and code availability {.unnumbered}

This manuscript's source, bibliography, and build tooling are public in the project repository. The deterministic focus protocol and the offline arbitration component described in Section 5.1 are part of the author's MATE/phlow codebases. The coordination event-log corpora are operational records of a working system and are not published; the evaluation fixtures for the trials of Section 6 are designed to be publishable with their results. The LoRA adapter of Section 5.2 is a local research artifact and is not distributed.

# References {.unnumbered}

::: {#refs}
:::
