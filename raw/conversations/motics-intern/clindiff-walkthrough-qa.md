# Clindiff Walkthrough — Anticipated Q&A (Priority Ordered)

**Scope: Block 1 only.** For Blocks 2-4 (STAR behavioural · role alignment · questions to ask), see **`round3-qa-blocks-2-3-4.md`**.

Companion file to `clindiff-walkthrough-prep.md`. After the 10-15 min walkthrough, expect targeted Q&A from the team. Questions below are ordered by **(probability asked) × (impact if fumbled)**.

Workflow:
1. Jayden writes a draft answer under each question (English).
2. AI returns utterance feedback in the usual format (grammar, articles, register, naturalness, content sharpness).
3. After iteration, answers get promoted into the prep doc / STAR bank as appropriate.

Cross-references:
- `background.md` — full Motics context (product, team, limitations, Audit KG pitch)
- `clindiff-walkthrough-prep.md` — walkthrough flow and known pushback topics
- `2026-04-20-session-03.md` — prior roleplay where James opened the DAG question

---

## Tier 1 — Must Nail
*High probability × high cost if fumbled. These four questions carry most of the interview's technical signal.*

---

### Q1a. "Where would you take clindiff next? What's missing?"

- **Asker:** James or Salinna (engineering-reflection framing, non-political — replaces the original "is this a prototype of Audit" framing, which was unrealistic given the team built Audit themselves)
- **Why it matters:** Sets up the technical follow-up tree. Each priority item is a deliberate hook into a Tier 2/3 question (Q3, Q5, Q6, Q14). Strong answer here means downstream questions feel like depth probes, not gotchas.
- **Key angles to cover:**
  - **Human-in-the-loop at multiple touchpoints**, not just one gate — at minimum: transcript structuring, template structuring, relevant section extraction, eval result on disputed cases
  - **Prose-to-JSON rule compiler** as the mechanism behind HITL #2 (template structuring) — clinician validates the compiled artifact
  - **Empirical validation of the 9-stage decomposition claim** — biggest unverified architectural claim, links to Q3
  - **Multi-judge ensemble** — single-judge today carries self-preference bias, links to Q5
  - **Infrastructure debt** as appendix — CI/CD, prompt-injection defences, SSE hardening (Q14 hook)
  - Closing logic: clinical grounding first, then architectural verification, then eval refinement
- **Prep status:** Drafted. Practice the four HITL touchpoints as a clean four-beat list — they're the load-bearing piece.
- **References:** `background.md` §1.5; `clindiff-walkthrough-prep.md` §3.4

#### Answer (draft)

Three things, in priority order.

First. Human-in-the-loop at multiple points. This is the biggest gap, and the most clinically consequential. Today's eval is LLM-only end-to-end. At minimum, four touchpoints need a clinician. Transcript structuring. Template structuring. Relevant section extraction. Final eval result on disputed cases. Each is a place where the LLM can hallucinate quietly and propagate downstream. The heaviest of the four is template structuring. It requires a prose-to-JSON rule compiler that emits machine-checkable constraints alongside the prose, so the clinician has something concrete to validate. Without HITL across all four, the rest is polish on an ungrounded system.

Second. Empirical validation of the 9-stage decomposition claim. The largest unverified architectural claim in the submission. I called it out in the limitations section. There's a one-week benchmark I'd run.

Third. Multi-judge ensemble. Today's eval is single-judge, which carries self-preference bias. Adding a second model and computing inter-judge agreement makes the signal more honest.

Beyond those three, infrastructure debt. CI/CD, prompt-injection defences, SSE hardening. Sprint shortcuts, not design choices. They'd come early in a proper buildout.

That's the order I'd work in. Clinical grounding first, then architectural verification, then eval refinement.

---

### Q1b. "If you extended clindiff toward Audit, what would change?"

- **Asker:** James or Harvinder (this is where the Audit KG vision actually lands — the original Q7 trojan-horse routing is still valid as a depth follow-up, but Q1b is the natural opener)
- **Why it matters:** Politically the most sensitive question in Block 1. If Audit has an internal owner, this answer must (a) demonstrate substantive vision, (b) not encroach on that owner's territory, (c) leave a collaborative posture. Get this right and Audit-related Tier 2/3 questions become invitations rather than tests.
- **Key angles to cover:**
  - **Layer 1 — Structural transfer:** clindiff's methodology (decomposition, dimension separation, controlled variance) carries directly to Audit. Same shape: note vs instructions / note vs regulation
  - **Layer 2 — Ontology + KG:** build regulatory ontology ourselves (NICE, CQC, GMC). LLM does entity extraction with ontology schema in context. Audit step is conformance check against ontology, not free LLM judgment. SNOMED CT as terminology backbone (NHS-mandated, OWL 2 EL)
  - **Layer 3 — Longitudinal care tracker:** SNOMED relations + NICE temporal pathways → system surfaces pending actions, imminent deadlines, overdue obligations. Reframes Audit from snapshot compliance into care-continuity layer
  - **Closing deference:** "before going deeper" — signals strong vision but defers to internal Audit owner before pushing further
- **Prep status:** Drafted. The three-layer structure is the load-bearing piece — practice the layer transitions so they sound deliberate, not stacked.
- **References:** `background.md` §1.7 (KG-augmented approach), §1.8 (pitch script), §1.9 (anticipated pushback)

#### Answer (draft)

A lot, actually. Three layers.

First. The structural transfer is direct. clindiff evaluates whether a note follows the template by interpreting the instructions correctly. Audit evaluates whether a note follows regulation by interpreting compliance requirements correctly. Same shape. Decomposition, dimension separation, controlled variance. Those all carry over.

Second. Where I'd go beyond clindiff. Bring in an ontology and a knowledge graph. We build the ontology of regulatory and clinical-pathway requirements ourselves. NICE, CQC, GMC. The LLM does what it's good at. Entity extraction from the note, with the ontology schema in context to constrain extraction. The audit step then becomes a conformance check between extracted entities and the ontology, not a free LLM judgment. SNOMED CT sits underneath as the terminology backbone, NHS-mandated and OWL-2-EL based.

Third. Beyond pointwise compliance. Reading SNOMED CT, I noticed it points past pure terminology. There's structure that captures relationships between diagnoses, actions, and indicated next steps. Layer NICE guidelines on top for temporal pathway logic, and Audit extends into a longitudinal care tracker. Each clinical event recorded against the ontology. The system surfaces pending actions, imminent deadlines, and overdue obligations. That turns Audit from a snapshot compliance check into an ongoing care-continuity layer.

That's my full take. Whether any of it lines up with where Audit is heading internally is something I'd want to hear from you before going deeper.

---

### Q2. "Why 9 stages instead of one monolithic LLM call?"

- **Asker:** James (AI Lead)
- **Why it matters:** Central architectural claim. James opened this exact question in Session 03 — expect it again, possibly reframed.
- **Key angles to cover:**
  - **Experiential** answer, not philosophical: consistent pattern across Gemini/GPT/Claude that models are sensitive to surrounding context
  - Decomposition lets you: retry failed stages, swap models per stage, optimise each independently
  - Trade-off honestly acknowledged: latency + unverified empirical gain (→ links to Q3)
- **Prep status:** Partially answered in Session 03. Tighten the delivery — the experiential framing is good but slightly rambling.
- **References:** `background.md` §1.2, §1.3 (philosophy 2: Decompose, Then Parallelize); `2026-04-20-session-03.md`

#### Answer (draft)

> "When I've worked with LLMs, I kept hitting the same pattern. Give a model too much context at once and performance gets harder to control. There's research on this too. Long-context degradation was a documented problem at the time. So when I built clindiff, keeping each stage's context focused felt like the right call.
>
> The decomposition also gives you practical leverage. If one stage fails, you retry that stage, not the whole run. You can swap models per stage, optimise each one independently. And from the user side, results come in as each stage completes, so you're not waiting on the full pipeline to see anything.
>
> That said, recent models have gotten much stronger on long context. So this assumption probably needs revisiting, not just restating. The empirical gain is unverified. That's the first benchmark I'd run if I had a week."

---

### Q3. "The decomposition claim is unverified. If I gave you one week, how would you benchmark it?"

- **Asker:** James
- **Why it matters:** This is the **biggest unvalidated claim** in the submission — and Jayden called it out himself in the Limitations section, which invites the follow-up. Answering cleanly turns a weakness into evidence of rigour.
- **Key angles to cover:**
  - **3-way comparison:** (a) current 9-stage Sonnet pipeline, (b) single-call Opus with full context, (c) single-call Sonnet monolithic
  - **Dataset:** N ≥ 20 fixed transcripts + templates
  - **Measure:** per-dimension scores, token cost, wall-clock latency
  - One-week timeline is genuinely feasible — show the plan, not hand-waving
- **Prep status:** Written out in prep doc §3.2 — rehearse the numbers so they come out fluently.
- **References:** `background.md` §1.5 (Empirical Validation of Decomposition Gains); `clindiff-walkthrough-prep.md` §3.2

#### Answer (draft)

The setup is a three-way comparison. Opus single-call as the monolithic baseline. Sonnet plus Haiku decomposed across the nine stages, which is the steel-manned version of the architectural claim. Sonnet single-call as a control, to separate raw model capability from the decomposition effect.

I'd run it on twenty transcript-template pairs. Not a statistical-power claim, just enough to be engineering-relevant.

One framing point. Decomposition was obviously right a year ago because long-context calls degraded badly. Opus and the recent Sonnet have closed most of that gap. So the assumption deserves a re-test, not a re-statement. That's why I'd run this benchmark now.

Three axes, in this order. Quality, cost, latency.

Quality first because it's the gate. If quality fails, cost and latency don't matter. Per-dimension scores against ground truth. Motics's labelled data ideal, otherwise manual review by me. And honestly, the steel-man might lose. Opus single-call has full context, no inter-stage information loss.

Cost next. Per-token call cost across the three conditions. Decomposition uses cheaper Haiku and Sonnet calls in volume. Opus is one expensive call.

Latency last. Wall-clock per run. Easiest to measure, most likely to hold.

If decomposed beats Opus on quality, the claim survives outright. If they tie on quality but decomposed wins on cost and latency, it survives as an efficiency story. If Sonnet single ties Opus single, then Opus's edge wasn't model capability, it was monolithic context. That'd be the most interesting result.

I don't already know which row I land in.

---

### Q4. "If I gave you a week to properly validate the DAG's retry logic, what does the test look like?"

- **Asker:** James (**left unanswered from Session 03** — will likely come back)
- **Why it matters:** Direct open thread from the prior session. Not answering it signals either forgot or ducked.
- **Key angles to cover:**
  - **Fault injection**: inject transient failures at each of the 9 stages, one at a time and in combinations
  - **Retry success criterion**: output identical to no-failure run within tolerance (define the tolerance — e.g. score delta ≤ ε on each dimension)
  - **Cost ceiling**: cap retries per run to prevent API-cost runaway (already called out as reason automatic retries were omitted)
  - **Failure modes to measure**: silent corruption (retry "succeeded" but output differs), infinite loop, partial state leakage between retries
- **Prep status:** Fresh territory — never answered. Highest-leverage prep item.
- **References:** `background.md` §1.5 (Unverified Retry Logic across 9 Stages); `clindiff-walkthrough-prep.md` §3.6; `2026-04-20-session-03.md`

#### Answer (draft)

The plan is fault injection on each of the nine stages. Single-stage first, then combinations. For each injection, I'd measure four things.

One. State and idempotency. Did the failed attempt leave anything behind. Partial writes, accumulator state, cache pollution that leaks into the retry.

Two. Downstream propagation. When L4 retries, do L7 and L8 actually consume the retried output, not the stale one.

Three. Failure detection accuracy. False negatives are the dangerous case. A stage returns garbage but gets marked successful. So I'd inject malformed outputs as well as exceptions.

Four. Cost ceiling. Confirm the retry counter actually fires. Test the race conditions on it. Make sure hitting the cap alerts cleanly instead of silently dropping the run.

And the failure data isn't just pass or fail. Each failure gets categorized. Schema-level, content-level, semantic. That taxonomy is what tells us whether to fix the orchestrator, the prompt, or the template itself. Without that, the test just tells you something broke. Not what to do about it.

For the pipeline's final output, I'd just check it's structurally valid and the scores fall in a normal range. Not a statistical comparison. Just a sanity check that the run completed coherently.

One honest caveat. This is integration-test scope. It validates the orchestrator, not whether the model itself is reliable on a given stage. That's a separate measurement problem.

---

## Tier 2 — Strongly Likely
*Secondary technical deep-dive territory. James will expect substantive answers, not sketches.*

---

### Q5. "Multi-Judge Ensemble — how would you compute inter-judge agreement?"

- **Asker:** James
- **Why it matters:** Tests whether the "Multi-Judge" idea is a real methodology or decorative. Imperial AI PhD will spot hand-waving.
- **Key angles to cover:**
  - Agreement metric choice: **Cohen's kappa** (two judges) or **Krippendorff's alpha** (N≥2, ordinal/interval data)
  - Why not raw correlation: doesn't penalise chance agreement
  - **Disagreement as signal**: high spread on a dimension = that dimension is genuinely ambiguous or stylistically contested, not noise to smooth over
  - Quantifying self-preference bias: each judge's average score on its own model family vs others — makes the bias observable
- **References:** `background.md` §1.5 (LLM-as-Judge Bias; Quantifying the bias itself)

#### Answer (draft)

> "I'd think about it in two parts.
>
> First, reliability. The more two judges agree, the more you can trust the evaluation. High disagreement on a dimension means the signal isn't stable — you shouldn't act on it until you understand why.
>
> Second, diagnosing the cause. Disagreement can come from two different places. One is bias — if one judge consistently scores higher or lower across the board, that's self-preference. You can see it by comparing each judge's average score when evaluating its own model family versus others. The other cause is the template itself. If two judges diverge on the same specific dimension across many notes, the prose rule for that dimension is probably underspecified. That's a template problem, not a judge problem.
>
> Separating those two is what makes multi-judge actually useful. Otherwise you just average away the disagreement and lose the signal."

---

### Q6. "The prose-to-JSON rule compiler — sketch the implementation."

- **Asker:** James
- **Why it matters:** This is the "central insight" from the limitations section. Claiming it as a fix but being unable to sketch it is a red flag.
- **Key angles to cover:**
  - Input: a prose rule with evaluative qualifiers ("include relevant past medical history", "summarize concisely")
  - Output: a JSON rule object preserving the original prose + attaching machine-checkable constraints (required fields, cardinality, value ranges, section locations)
  - Mechanism: LLM as translator, structured output schema, **mandatory HITL approval gate on the compiled schema** (current weak point)
  - Partial AST extension from section-level to field-rule-level naturally converges with this
- **References:** `background.md` §1.5 (Prose → JSON Rule Object Compilation; Partial AST)

#### Answer (draft)

> "Two kinds of rules, two mechanisms. Structural and documentation rules compile to boolean conditions — that's the compiler. Clinical content rules need an ontology layer on top. I'll cover the compiler.
>
> Two steps. The compiler reads a template section and extracts what needs checking. A rule like 'document medication changes with dosage and rationale' becomes three conditions: medication change stated, dosage specified, rationale given. The clinician approves the compiled conditions before anything runs.
>
> Then each statement in the note is checked against those conditions. The LLM answers true or false per condition — never writes a judgment, just fills a form whose shape it can see. Hallucination stays low for the same reason.
>
> The HITL gate sits on the compiled conditions, not on the LLM's output. Fix it there and every run after is deterministic.
>
> Clinical content validation is the other half — the ontology layer."

---

### Q7. "Clinical Ontology as a next step — how would you actually build it?"

- **Asker:** James or Harvinder (natural bridge to Audit KG pitch)
- **Why it matters:** This is a **trojan-horse question** — answering well lets the Audit KG + SNOMED CT pitch land without a forced transition.
- **Key angles to cover:**
  - Not "build an ontology from scratch" — **SNOMED CT** is NHS-mandated and already there
  - LLM entity extraction → SNOMED mapping → Cypher query against regulatory rules → query trace as audit trail
  - **Neo4j over a dedicated RDF reasoner**: property edges model conditional applicability cleanly; one graph engine unifies inference + validation + rule logic that traditional OWL+SHACL+SWRL stacks split across three. Considered ELK (correct EL profile fit), but pre-materialising EL inference at import time reproduces reasoner output without a standalone process.
  - **Hands-on grounding**: working through this exact RDF→Neo4j mapping in side project `no-more-rdf` — answer comes from doing it, not reading about it
- **Prep status:** Well-prepped — this is the core round-3 pitch. Practice landing it in <90 seconds.
- **References:** `background.md` §1.7 (KG-augmented approach), §1.8 (pitch script), §1.9 (anticipated pushback)

#### Answer (draft)

> "Starting point isn't building from scratch. SNOMED CT already exists, NHS-mandated, ships in RDF. Migrates straight into Neo4j.
>
> On the substrate choice — I'm working through this exact problem in a side project, `no-more-rdf`, mapping RDF, OWL, SHACL, and SWRL onto Neo4j systematically. Short version: property edges model regulatory conditional applicability cleanly, Cypher beats SPARQL for readability, and one graph engine handles inference, validation, and rule logic that traditional stacks split across three.
>
> For SNOMED specifically, EL profile inference — IS_A closure, role groups, propertyChainAxiom — gets pre-materialised at import time. That replicates an ELK-style reasoner's output as a one-time cost, so runtime is fast graph lookups, not deep traversals.
>
> Pipeline on top: LLM extracts clinical entities from the note, maps to SNOMED, compliance is a Cypher query against regulatory rules encoded as edges. Query trace is the audit trail.
>
> The same graph extends — clinical events recorded against SNOMED, NICE temporal pathways layered on top, and Audit becomes a longitudinal care tracker, not just a snapshot check."

---

### Q8. "What would you do differently now, knowing what you know?"

- **Asker:** Salinna (softer engineering-reflection question)
- **Why it matters:** Tests self-awareness without defensiveness. Picking the *right* limitations to surface shows judgment.
- **Key angles to cover:** Pick **2–3 from:**
  - Empirical validation of decomposition (the biggest unvalidated claim — shows you know which limitation is top)
  - Multi-Judge Ensemble (avoid single-judge self-preference)
  - Clinical HITL for ground truth (current eval is purely LLM-driven)
  - Mandatory HITL gate on compiler output
- **Prep status:** Mental model exists — practice the shortlist so it comes out crisp, not rambling.
- **References:** `background.md` §1.5; `clindiff-walkthrough-prep.md` §3.4

#### Answer (draft)

> "Three calls I'd revisit.
>
> The decomposition assumption. I built nine stages because long-context was unreliable when I started. Recent Sonnet and Opus have closed most of that gap. I shipped the architecture preserving an assumption I should have re-tested. That's the first one.
>
> The HITL gate on the compiler — I shipped it as optional. That was a pragmatism call, get the pipeline running first. Knowing how confidently LLMs hallucinate plausible-looking schemas, I'd make clinician approval mandatory from day one. The gate isn't a feature, it's a clinical-safety prerequisite.
>
> And single-judge eval. I knew about self-preference bias going in and accepted the cost for the sprint. Adding a second judge and computing inter-judge agreement would have made the eval more honest from the start."

---

## Tier 3 — Medium Likelihood
*Likely but less central. Have a ready answer; don't over-rehearse at the expense of Tier 1/2.*

---

### Q9. "Why k3s self-hosted instead of AWS/GCP? Production scaling concerns?"

- **Asker:** Salinna
- **Why it matters:** Surfaces whether Jayden made the choice deliberately or by default.
- **Key angles to cover:**
  - Private take-home, security-conscious stack, full control
  - Not a production claim — hardening is a stated prerequisite before any public deployment
  - If pushed on production path: GCP GKE or AWS EKS for a real deployment; k3s was fit-for-purpose for a 4-day private sprint
- **References:** `background.md` §1.4 (Tech Stack), §1.5 (Infrastructure Debt)

#### Answer (draft)

> "Honest answer — I already had a k3s cluster running on a free-tier VM for other side projects. For a four-day take-home, using what was already set up was the obvious call. Not an architectural decision.
>
> For a real Motics deployment, GKE or EKS. The DAG and orchestration code are container-portable, so the migration is infra config, not an application rewrite." "How would you detect hallucinations in production, not just in eval?"

- **Asker:** James (or Harvinder from clinical-safety angle)
- **Why it matters:** Shifts the discussion from batch eval to runtime guardrails — tests production thinking.
- **Key angles to cover:**
  - Promote L5 Faithfulness from post-hoc metric to runtime check: every generated entity verified against the transcript
  - Low-confidence faithfulness flags → block-and-escalate (HITL), not auto-pass
  - Entity-level confidence thresholds, not document-level
  - Separate path for "can't verify" vs "verified false" — different downstream action
- **References:** `background.md` §1.2 (L5 Faithfulness), §1.5 (Compiler Hallucination & Missing HITL)

#### Answer (draft)

> "Eval-time compares against ground truth. Production has no ground truth — the question is what to check against the source itself.
>
> Part of the mechanism is already there. clindiff today annotates each sentence in the generated note with a Faithfulness verdict against the transcript. Grounded sentences are marked inline, unsupported claims surface in a separate section. So the per-claim attribution exists at eval time.
>
> Two changes for production. One, runtime instead of post-hoc — the gate runs before the note ships, and unsupported claims block delivery rather than appear on a report. Two, a third verdict state. Today the check is effectively binary, supported or not. Production needs a 'cannot verify' bucket in between — ambiguous evidence flagged and routed to the clinician with the source span attached, not auto-passed and not blocked.
>
> That third bucket is what production thinking actually requires. Under-blocking is clinical risk, over-blocking destroys workflow trust. Different escalation, different tolerance.
>
> Honest caveat — the confidence thresholds need calibration on real Motics data before any of this runs."

---

### Q11. "What does Human-in-the-Loop look like in this system specifically — not as a general idea?"

- **Asker:** Harvinder (clinical safety framing) or Salinna (engineering framing)
- **Why it matters:** Avoids the trap of treating HITL as a buzzword. Harvinder cares about *where the clinician sits* in the flow.
- **Key angles to cover:**
  - **Compiler gate**: LLM-generated schemas must be clinician-approved before the eval pipeline uses them (currently optional, should be mandatory)
  - **Faithfulness flags**: low-confidence entity extractions routed to clinician review, not auto-accepted
  - **Disagreement loop**: when clinician overrides the system, the delta feeds back into template or schema refinement (virtuous cycle)
  - Honest framing: current clindiff is LLM-only — HITL is a gap the Limitations section names explicitly
- **References:** `background.md` §1.5 (Clinical HITL Necessity; Compiler Hallucination)

#### Answer (draft)

> "From the clinician's view, three places they show up.
>
> Upstream sign-off. Before eval runs, clinician approves the structured artifacts — normalized transcript, structured template, compiled boolean conditions per rule. Reviewed when something upstream changes, not every run. These gates stop hallucination from propagating downstream.
>
> Disputed cases during eval. Low-confidence Faithfulness flags route to a queue. Clinician sees the note statement, the LLM's true-or-false per condition, and the rule. One-click confirm or override.
>
> Final sign-off. Two things sit with the clinician end of pipeline — validating that any system-flagged feedback is itself correct, and signing off the note before it leaves the system. Who *first* surfaces a prose-ambiguous rule depends on the deployment — admin or clinician — but those two roles are always the clinician's.
>
> Honest caveat — current clindiff is LLM-only end-to-end. This is the design, not what's shipped."

---

### Q12. "Correctness / Completeness / Compliance / Quality / Nuance — why these five? Why not standard NLG metrics (ROUGE, BERTScore)?"

- **Asker:** James
- **Why it matters:** Tests whether the metric split is principled or ad hoc.
- **Key angles to cover:**
  - ROUGE/BERTScore measure lexical overlap — not faithfulness, not compliance, not clinical quality
  - Split follows the **Control the Variables** philosophy: deterministic checks separated from LLM-as-judge signals
  - Correctness (precision / hallucination) + Completeness (recall) + Compliance (template rules) are **the three** that map onto the transcript-note problem structurally
  - Quality + Nuance are catch-alls — named honestly as less critical (major issues already caught upstream)
- **References:** `background.md` §1.3 (Philosophy 3: Control the Variables); `clindiff-walkthrough-prep.md` §1.1 Loom script §4

#### Answer (draft)

> "Two grounding sources. The five metrics came from reading CQC documentation — what UK regulators actually evaluate clinical notes against — and from Motics's own product positioning on what scribe quality means. So the metrics are shaped by deployment context, not pulled from NLP literature.
>
> Correctness and Completeness map to precision and recall on the transcript-to-note problem. Correctness — did the note invent or distort anything. Completeness — did the note miss what mattered. These are the foundational signal-detection metrics for any extraction-and-summarisation problem. They sit underneath everything else.
>
> Compliance is template-rule conformance. CQC framing pushed it in — it's the closest thing to what Audit will eventually do at scale.
>
> Quality and Nuance are honest catch-alls. By the time they run, Correctness, Completeness, and Compliance have already caught the major issues. They're stylistic safety nets, named as such.
>
> On ROUGE and BERTScore — they measure lexical overlap with a reference summary. Clinical scribes don't have a single correct paraphrase. Two notes can be lexically very different and both correct, or lexically similar with one of them hallucinated. The metric has to operate on facts and rule conformance, not surface form."

---

## Tier 4 — Contextual / Opportunistic
*Lower probability but worth having a 30-second answer ready. Don't over-prepare.*

---

### Q13. Harvinder on clinical consequence: "If this misjudges a note, what's the clinical impact?"

- **Asker:** Harvinder
- **Key angles:** Define the error mode — false-positive (flags good note) vs false-negative (misses a real issue). False-negative is the clinically dangerous case. Risk is contained by (a) clindiff being *eval*, not gatekeeper, (b) clinician-in-loop, (c) audit trail preserves decision path so errors are traceable and correctable.

#### Answer (draft)

> "Two error modes. False-positive — flags a good note as failing. Annoying, costs clinician time, fixable in the loop. False-negative — misses a real issue and lets the note pass. That's the clinically dangerous one.
>
> Three things contain that risk. clindiff is evaluation, not a gatekeeper — the clinician still owns the final sign-off. The HITL touchpoints upstream and on disputed cases mean errors get caught before they propagate. And the audit trail preserves the decision path on every claim, so when something is wrong, it's traceable and correctable, not opaque."

---

### Q14. Salinna on engineering tradeoffs: "4-day sprint — what did you cut? What would you keep even in a 4-week version?"

- **Asker:** Salinna
- **Key angles:** Cut: CI/CD, test coverage, prompt-injection defences, real-time SSE hardening. Keep (even in 4 weeks): the 9-stage DAG shape, JSON-first schemas, metric separation — these are design choices, not sprint shortcuts.

#### Answer (draft)

> "Cut were the things a four-day private sprint can defer — CI/CD, broad test coverage, prompt-injection defences, real-time SSE hardening. Sprint shortcuts, all called out in the limitations section.
>
> Keep, even in a four-week version — the nine-stage DAG shape, JSON-first schemas at every interface, the metric separation between deterministic checks and LLM-as-judge signals. These aren't sprint shortcuts, they're design choices. They'd be the same architecture in week one of a longer build.
>
> The split is the point. A four-week version isn't a four-day version with more polish. It's the same architecture with the cut items added back."

---

### Q15. On the Simulation view: "Why single-model? What does variance actually tell us?"

- **Asker:** James (if we get to the simulation screen in the walkthrough)
- **Key angles:** Single-model isolates **instability vs template-prose ambiguity** — the two main variance sources. Multi-model would confound with cross-model preference bias. Appendix has full reasoning. Variance localises *which rules* in the template are prose-ambiguous and need compilation.

#### Answer (draft)

> "Two variance sources matter. Model instability — same prompt, different runs, different answers. Template prose ambiguity — the rule itself is underspecified, so any judge will diverge on it.
>
> Single-model isolates those two. Multi-model would confound them with a third — cross-model preference bias — and you can't tell which is which.
>
> What variance actually tells us: when single-model variance is high on a specific rule, the rule's prose is ambiguous, not the model. That's a direct signal — the rule needs compilation into boolean conditions. It's the same hook into the prose-to-JSON compiler I mentioned earlier."

---

## Practice Notes

- **Start with Q4** (retry logic) — it's the most exposed gap and genuinely unanswered.
- **Then Q3** (decomposition benchmark) — the most named limitation. Fluent delivery here is disproportionately valuable.
- **Q1b is now where the Audit KG vision lands directly** — Q7 becomes a depth follow-up if asked, not the main vehicle. Q1a precedes it as the internal-extension roadmap and seeds Q3/Q5/Q6/Q14 hooks.
- Target length per answer: **60–120 seconds spoken**. Any longer is a monologue; any shorter and it looks like a pre-canned soundbite.
- After drafting, mark for review: which answers use **articles correctly** (see [[missing-articles]]), which slip into **casual register** (see [[informal-register-in-interview]]).
