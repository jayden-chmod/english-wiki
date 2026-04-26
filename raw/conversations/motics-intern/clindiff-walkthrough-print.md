# Clindiff Walkthrough — Q&A Print Sheet

**Date:** 2026-04-25
**Scope:** Round 3 Block 1 (clindiff walkthrough + Q&A)
**Target length per spoken answer:** 60–120 sec (Q3/Q4 may run longer)

---

## TIER 1 — MUST NAIL

### Q1a. Where would you take clindiff next? What's missing?
**Asker:** James / Salinna (engineering-reflection framing, non-political)

**Key angles:**
- HITL across 4 touchpoints: transcript structuring, template structuring, relevant section extraction, eval result on disputed cases
- Compiler embedded under HITL #2 (mechanism for template structuring) — Q6 hook
- Empirical validation of decomposition — Q3 hook
- Multi-judge ensemble — Q5 hook
- Infrastructure debt as appendix — Q14 hook

**Answer draft:**

Three things, in priority order.

First. Human-in-the-loop at multiple points. This is the biggest gap, and the most clinically consequential. Today's eval is LLM-only end-to-end. At minimum, four touchpoints need a clinician. Transcript structuring. Template structuring. Relevant section extraction. Final eval result on disputed cases. Each is a place where the LLM can hallucinate quietly and propagate downstream. The heaviest of the four is template structuring. It requires a prose-to-JSON rule compiler that emits machine-checkable constraints alongside the prose, so the clinician has something concrete to validate. Without HITL across all four, the rest is polish on an ungrounded system.

Second. Empirical validation of the 9-stage decomposition claim. The largest unverified architectural claim in the submission. I called it out in the limitations section. There's a one-week benchmark I'd run.

Third. Multi-judge ensemble. Today's eval is single-judge, which carries self-preference bias. Adding a second model and computing inter-judge agreement makes the signal more honest.

Beyond those three, infrastructure debt. CI/CD, prompt-injection defences, SSE hardening. Sprint shortcuts, not design choices. They'd come early in a proper buildout.

That's the order I'd work in. Clinical grounding first, then architectural verification, then eval refinement.

---

### Q1b. If you extended clindiff toward Audit, what would change?
**Asker:** James / Harvinder (where the Audit KG vision actually lands)

**Key angles:**
- Structural transfer direct (same shape: note vs instructions / note vs regulation)
- Ontology + KG layer: build regulatory ontology (NICE / CQC / GMC), SNOMED CT as terminology backbone (NHS-mandated, OWL 2 EL)
- LLM does extraction; audit step is conformance check against ontology, not free judgment
- Longitudinal extension: SNOMED relations + NICE temporal pathways → pending / imminent / overdue actions
- Audit owner deference: "before going deeper" closing

**Answer draft:**

A lot, actually. Three layers.

First. The structural transfer is direct. clindiff evaluates whether a note follows the template by interpreting the instructions correctly. Audit evaluates whether a note follows regulation by interpreting compliance requirements correctly. Same shape. Decomposition, dimension separation, controlled variance. Those all carry over.

Second. Where I'd go beyond clindiff. Bring in an ontology and a knowledge graph. We build the ontology of regulatory and clinical-pathway requirements ourselves. NICE, CQC, GMC. The LLM does what it's good at. Entity extraction from the note, with the ontology schema in context to constrain extraction. The audit step then becomes a conformance check between extracted entities and the ontology, not a free LLM judgment. SNOMED CT sits underneath as the terminology backbone, NHS-mandated and OWL-2-EL based.

Third. Beyond pointwise compliance. Reading SNOMED CT, I noticed it points past pure terminology. There's structure that captures relationships between diagnoses, actions, and indicated next steps. Layer NICE guidelines on top for temporal pathway logic, and Audit extends into a longitudinal care tracker. Each clinical event recorded against the ontology. The system surfaces pending actions, imminent deadlines, and overdue obligations. That turns Audit from a snapshot compliance check into an ongoing care-continuity layer.

That's my full take. Whether any of it lines up with where Audit is heading internally is something I'd want to hear from you before going deeper.

---

### Q2. Why 9 stages instead of one monolithic LLM call?
**Asker:** James (AI Lead)

**Key angles:**
- Experiential, not philosophical: consistent pattern across Gemini/GPT/Claude that models are sensitive to surrounding context
- Decomposition lets you: retry failed stages, swap models per stage, optimise each independently
- Trade-off honestly acknowledged: latency + unverified empirical gain (links to Q3)

**Answer draft:**
_[pending]_

---

### Q3. The decomposition claim is unverified. If I gave you one week, how would you benchmark it?
**Asker:** James

**Answer draft:**

The setup is a three-way comparison. Opus single-call as the monolithic baseline. Sonnet plus Haiku decomposed across the nine stages, which is the steel-manned version of the architectural claim. Sonnet single-call as a control, to separate raw model capability from the decomposition effect.

N is twenty transcript-template pairs. Engineering-relevant rather than a statistical-power claim.

One framing point. Decomposition was obviously right a year ago because long-context calls degraded badly. Opus and the recent Sonnet have closed most of that gap. So the assumption deserves a re-test, not a re-statement. That's why I'd run this benchmark now.

Three axes, in this order. Quality, cost, latency.

Quality first because it's the gate. If quality fails, cost and latency don't matter. Per-dimension scores against ground truth. Motics's labelled data ideal, otherwise manual review by me. And honestly, the steel-man might lose. Opus single-call has full context, no inter-stage information loss.

Cost next. Per-token call cost across the three conditions. Decomposition uses cheaper Haiku and Sonnet calls in volume. Opus is one expensive call.

Latency last. Wall-clock per run. Easiest to measure, most likely to hold.

If decomposed beats Opus on quality, the claim survives outright. If they tie on quality but decomposed wins on cost and latency, it survives as an efficiency story. If Sonnet single ties Opus single, then Opus's edge wasn't model capability, it was monolithic context. That'd be the most interesting result.

I don't already know which row I land in.

---

### Q4. If I gave you a week to properly validate the DAG's retry logic, what does the test look like?
**Asker:** James (left unanswered from Session 03)

**Answer draft:**

The plan is fault injection on each of the nine stages. Single-stage first, then combinations. For each injection, I'd measure four things.

One. State and idempotency. Did the failed attempt leave anything behind. Partial writes, accumulator state, cache pollution that leaks into the retry.

Two. Downstream propagation. When L4 retries, do L7 and L8 actually consume the retried output, not the stale one.

Three. Failure detection accuracy. False negatives are the dangerous case. A stage returns garbage but gets marked successful. So I'd inject malformed outputs as well as exceptions.

Four. Cost ceiling. Confirm the retry counter actually fires. Test the race conditions on it. Make sure hitting the cap alerts cleanly instead of silently dropping the run.

And the failure data isn't just pass or fail. Each failure gets categorized. Schema-level, content-level, semantic. That taxonomy is what tells us whether to fix the orchestrator, the prompt, or the template itself. Without that, the test just tells you something broke. Not what to do about it.

For the pipeline's final output, I'd just check it's structurally valid and the scores fall in a normal range. Not a statistical comparison. Just a sanity check that the run completed coherently.

One honest caveat. This is integration-test scope. It validates the orchestrator, not whether the model itself is reliable on a given stage. That's a separate measurement problem.

---

## TIER 2 — STRONGLY LIKELY

### Q5. Multi-Judge Ensemble — how would you compute inter-judge agreement?
**Asker:** James

**Key angles:**
- Cohen's kappa (two judges) or Krippendorff's alpha (N≥2, ordinal/interval)
- Why not raw correlation: doesn't penalise chance agreement
- Disagreement as signal: high spread on a dimension = genuinely ambiguous, not noise
- Quantifying self-preference bias: each judge's avg score on its own model family vs others

**Answer draft:**
_[pending]_

---

### Q6. The prose-to-JSON rule compiler — sketch the implementation.
**Asker:** James

**Key angles:**
- Input: prose rule with evaluative qualifiers ("include relevant past medical history")
- Output: JSON rule object preserving prose + machine-checkable constraints (required fields, cardinality, value ranges, section locations)
- Mechanism: LLM as translator, structured output schema, **mandatory HITL approval gate** on compiled schema
- Partial AST extension from section-level to field-rule-level converges with this

**Answer draft:**
_[pending]_

---

### Q7. Clinical Ontology as a next step — how would you actually build it?
**Asker:** James or Harvinder (trojan-horse for Audit KG pitch)

**Key angles:**
- Not "build from scratch" — **SNOMED CT** is NHS-mandated and already there
- LLM entity extraction → SNOMED mapping → OWL representation → DL queries
- Reasoner: **ELK** (handles OWL 2 EL, SNOMED's profile) — not HermiT/Pellet at this scale
- Regulatory rules as DL queries (GMC, NICE, GDPR) — query trace = audit trail

**Answer draft:**
_[pending]_

---

### Q8. What would you do differently now, knowing what you know?
**Asker:** Salinna

**Key angles (pick 2–3):**
- Empirical validation of decomposition (the biggest unvalidated claim — shows you know the top limitation)
- Multi-Judge Ensemble (avoid single-judge self-preference)
- Clinical HITL for ground truth (current eval is purely LLM-driven)
- Mandatory HITL gate on compiler output

**Answer draft:**
_[pending]_

---

## TIER 3 — MEDIUM LIKELIHOOD

### Q9. Why k3s self-hosted instead of AWS/GCP? Production scaling concerns?
**Asker:** Salinna

**Key angles:**
- Private take-home, security-conscious stack, full control
- Not a production claim — hardening is a stated prerequisite before any public deployment
- Production path: GCP GKE or AWS EKS; k3s was fit-for-purpose for a 4-day private sprint

**Answer draft:**
_[pending]_

---

### Q10. How would you detect hallucinations in production, not just in eval?
**Asker:** James (or Harvinder, clinical safety angle)

**Key angles:**
- Promote L5 Faithfulness from post-hoc metric to runtime check: every entity verified against transcript
- Low-confidence faithfulness flags → block-and-escalate (HITL), not auto-pass
- Entity-level confidence thresholds, not document-level
- Separate path for "can't verify" vs "verified false" — different downstream action

**Answer draft:**
_[pending]_

---

### Q11. What does Human-in-the-Loop look like in this system specifically?
**Asker:** Harvinder (clinical) or Salinna (engineering)

**Key angles:**
- **Compiler gate**: LLM-generated schemas must be clinician-approved before pipeline uses them (currently optional, should be mandatory)
- **Faithfulness flags**: low-confidence entity extractions routed to clinician review
- **Disagreement loop**: clinician overrides feed back into template/schema refinement
- Honest framing: current clindiff is LLM-only — HITL is a named gap

**Answer draft:**
_[pending]_

---

### Q12. Correctness / Completeness / Compliance / Quality / Nuance — why these five? Why not ROUGE / BERTScore?
**Asker:** James

**Key angles:**
- ROUGE/BERTScore measure lexical overlap — not faithfulness, not compliance, not clinical quality
- Split follows "Control the Variables" philosophy: deterministic checks separated from LLM-as-judge signals
- Correctness (precision/hallucination) + Completeness (recall) + Compliance (template rules) = the three that map onto the transcript-note structure
- Quality + Nuance are catch-alls — named honestly as less critical (major issues caught upstream)

**Answer draft:**
_[pending]_

---

## TIER 4 — CONTEXTUAL / OPPORTUNISTIC

### Q13. (Harvinder, clinical consequence) If this misjudges a note, what's the clinical impact?

**Key angles:**
- Define error mode: false-positive (flags good note) vs false-negative (misses real issue)
- False-negative is the clinically dangerous case
- Risk contained by: (a) clindiff is *eval*, not gatekeeper, (b) clinician-in-loop, (c) audit trail preserves decision path

**Answer draft:**
_[pending]_

---

### Q14. (Salinna, engineering tradeoffs) 4-day sprint — what did you cut? What would you keep even in a 4-week version?

**Key angles:**
- Cut: CI/CD, test coverage, prompt-injection defences, real-time SSE hardening
- Keep (even in 4 weeks): the 9-stage DAG shape, JSON-first schemas, metric separation — design choices, not sprint shortcuts

**Answer draft:**
_[pending]_

---

### Q15. (Simulation view) Why single-model? What does variance actually tell us?

**Key angles:**
- Single-model isolates **instability vs template-prose ambiguity** — the two main variance sources
- Multi-model would confound with cross-model preference bias
- Variance localises *which rules* in the template are prose-ambiguous and need compilation

**Answer draft:**
_[pending]_

---

## PRACTICE PRIORITY ORDER

1. **Q4** (retry logic) — most exposed, genuinely unanswered from Session 03
2. **Q3** (decomposition benchmark) — most-named limitation, fluent delivery disproportionately valuable
3. **Q1b** — Audit KG vision now lands directly here; Q7 becomes depth follow-up if asked
4. **Q1a** — internal extension roadmap; sets up Q3/Q5/Q6/Q14 follow-ups

## DELIVERY REMINDERS

- 60–120 sec spoken per answer (Q3/Q4 may run ~2 min)
- Articles correct (a/an/the)
- No casual register slips
- Opener of Q4: jump straight to test design (no meta-framing)
- Quality-first ordering in Q3 — methodological signal
- Q1a: internal-only roadmap; each item is a deliberate hook into Q3 / Q5 / Q6 / Q14
- Q1b: Audit KG vision lands here; close with "before going deeper" to defer to Audit owner
- Q7: now a depth follow-up to Q1b, not the main vehicle
