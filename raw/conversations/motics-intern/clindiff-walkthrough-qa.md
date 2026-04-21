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

### Q1. "How does clindiff relate to Audit Agent? Is this a prototype of Audit?"

- **Asker:** Salinna or Harvinder (possibly James — politically loaded for him if he owns Audit)
- **Why it matters:** Framing clindiff as "Audit prototype" is technically inaccurate AND politically risky (encroaches on whoever owns Audit). Getting this wrong can derail the role-assignment conversation later.
- **Key angles to cover:**
  - clindiff = Scribe QA (transcript → note fidelity)
  - Audit = compliance judgment (note vs UK regulation) — different problem class
  - Bridge: clindiff's L5 Faithfulness *could* feed Audit, but they are separate systems
  - Natural handoff to Audit KG + SNOMED CT pitch (§1.7–1.9 of background)
- **Prep status:** Well-prepped — see `background.md` §1.6. Practice the transition into the Audit KG pitch.
- **References:** `background.md` §1.6, §1.7, §1.8

#### Answer (draft)
_[fill in]_

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
_[fill in]_

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
_[fill in]_

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
_[fill in]_

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
_[fill in]_

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
_[fill in]_

---

### Q7. "Clinical Ontology as a next step — how would you actually build it?"

- **Asker:** James or Harvinder (natural bridge to Audit KG pitch)
- **Why it matters:** This is a **trojan-horse question** — answering well lets the Audit KG + SNOMED CT pitch land without a forced transition.
- **Key angles to cover:**
  - Not "build an ontology from scratch" — **SNOMED CT** is NHS-mandated and already there
  - LLM entity extraction → SNOMED mapping → OWL representation → DL queries
  - Reasoner choice: **ELK** (handles OWL 2 EL, which is SNOMED's profile) — not HermiT/Pellet for this scale
  - Regulatory rules as DL queries (GMC, NICE, GDPR) — the query trace becomes the audit trail
- **Prep status:** Well-prepped — this is the core round-3 pitch. Practice landing it in <90 seconds.
- **References:** `background.md` §1.7 (KG-augmented approach), §1.8 (pitch script), §1.9 (anticipated pushback)

#### Answer (draft)
_[fill in]_

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
_[fill in]_

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
_[fill in]_

---

### Q10. "How would you detect hallucinations in production, not just in eval?"

- **Asker:** James (or Harvinder from clinical-safety angle)
- **Why it matters:** Shifts the discussion from batch eval to runtime guardrails — tests production thinking.
- **Key angles to cover:**
  - Promote L5 Faithfulness from post-hoc metric to runtime check: every generated entity verified against the transcript
  - Low-confidence faithfulness flags → block-and-escalate (HITL), not auto-pass
  - Entity-level confidence thresholds, not document-level
  - Separate path for "can't verify" vs "verified false" — different downstream action
- **References:** `background.md` §1.2 (L5 Faithfulness), §1.5 (Compiler Hallucination & Missing HITL)

#### Answer (draft)
_[fill in]_

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
_[fill in]_

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
_[fill in]_

---

## Tier 4 — Contextual / Opportunistic
*Lower probability but worth having a 30-second answer ready. Don't over-prepare.*

---

### Q13. Harvinder on clinical consequence: "If this misjudges a note, what's the clinical impact?"

- **Asker:** Harvinder
- **Key angles:** Define the error mode — false-positive (flags good note) vs false-negative (misses a real issue). False-negative is the clinically dangerous case. Risk is contained by (a) clindiff being *eval*, not gatekeeper, (b) clinician-in-loop, (c) audit trail preserves decision path so errors are traceable and correctable.

#### Answer (draft)
_[fill in]_

---

### Q14. Salinna on engineering tradeoffs: "4-day sprint — what did you cut? What would you keep even in a 4-week version?"

- **Asker:** Salinna
- **Key angles:** Cut: CI/CD, test coverage, prompt-injection defences, real-time SSE hardening. Keep (even in 4 weeks): the 9-stage DAG shape, JSON-first schemas, metric separation — these are design choices, not sprint shortcuts.

#### Answer (draft)
_[fill in]_

---

### Q15. On the Simulation view: "Why single-model? What does variance actually tell us?"

- **Asker:** James (if we get to the simulation screen in the walkthrough)
- **Key angles:** Single-model isolates **instability vs template-prose ambiguity** — the two main variance sources. Multi-model would confound with cross-model preference bias. Appendix has full reasoning. Variance localises *which rules* in the template are prose-ambiguous and need compilation.

#### Answer (draft)
_[fill in]_

---

## Practice Notes

- **Start with Q4** (retry logic) — it's the most exposed gap and genuinely unanswered.
- **Then Q3** (decomposition benchmark) — the most named limitation. Fluent delivery here is disproportionately valuable.
- **Q1 and Q7 pair naturally** — practicing Q1 sets up the transition to the Audit KG pitch via Q7.
- Target length per answer: **60–120 seconds spoken**. Any longer is a monologue; any shorter and it looks like a pre-canned soundbite.
- After drafting, mark for review: which answers use **articles correctly** (see [[missing-articles]]), which slip into **casual register** (see [[informal-register-in-interview]]).
