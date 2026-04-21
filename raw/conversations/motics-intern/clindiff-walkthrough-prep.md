# Clindiff Walkthrough — Round 3 Block 1 Prep

## Purpose
Preparation for the "brief in-person walkthrough of the take-home assessment + Q&A" portion of the Motics 3rd round interview (confirmed format 2026-04-20).

Target length: 10-15 min walkthrough + targeted Q&A. The team has already read the submission.

---

## 1. Source Material

### 1.1 Existing Loom Demo Script

Jayden previously recorded a Loom video demo with the following script:

**1. Introduction**
> "Hi, I'm Jayden. This is Clindiff, a project I built for the take-home assignment."

**2. Home & Generation**
> "This is the Generate screen. I've pre-loaded some samples in the top-right dropdown, but I'll upload a TXT file as required for the assignment. I'll set the model to a smaller one for this demo. I want to see how the system handles potential errors and hallucinations."

**3. Navigation & Loading**
> "Now I hit generate, and it navigates to the compare screen. You can see the preprocessing status at the top. While it's loading, let's look at the sections below. This is the note section, and right below it is the evaluation area."

**4. Explaining Metrics**
- **Correctness**: "This measures Precision. It's essentially a hallucination check to see if the information is accurate."
- **Completeness**: "This is about Recall. It checks if the model captured all the necessary dialogue based on the template."
- **Compliance**: "This checks if the model followed the specific rules in the template. You can see the results grouped by each rule."
- **Quality & Nuance**: "Quality provides a qualitative assessment for additional criteria. Nuance focuses on how the same content is expressed differently. Since major issues are already caught by the other metrics, you might not see significant results here very often."

**5. Results & Evidence**
> "Now the notes are generated. The default is the Inspect view. Notice how the question marks on the left change in real-time as each evaluation result arrives. We also support a Markdown preview mode. Let's fast-forward to the results. You can also navigate to the evidence for each note and the reasoning behind the judgments. This is a feature built for explainability. Clicking it will take you directly to that spot and highlight the evidence for you."

**6. Simulation**
> "Next is the Simulation screen. I'll run 10 trials with a batch size of 3. I intentionally chose a single-model simulation, and I've detailed my reasoning for this in the Appendix. As you can see, it's processing in batches of three. Low scores or high variance here usually point to two things: Model instability or vague instructions in the template. To address this, I've included a feature for LLM feedback on the template, though the backend is not yet implemented. You can also compare simulation runs. Both regular and simulation runs can be compared on each page, even if they share the same template and transcript."

**7. Templates, Scripts & Philosophy**
> "Finally, we have the Template and Transcript screens. I believe that in fields where full automation is difficult or costly, Human-in-the-loop is essential. It's not just about immediate performance, but also about accumulating high-quality data for the future. These screens can be placed before note generation to check whether the transcript was well preprocessed and the template well structured."

---

## 2. Loom → In-Person Adaptation

The Loom script is the right skeleton but the delivery and structure need to change for the live interview.

| Loom format | In-person adjustment |
|---|---|
| "Hi, I'm Jayden" | Skip — they know you |
| "Let's fast-forward to the results" | Live demo can't fast-forward. Options: (a) pre-generate before the session so results are already loaded, (b) start a generate run and use the wait time to explain design philosophy |
| Screen-by-screen demo tone | More conversational. Narrate the *why* alongside the *what* |
| Philosophy at the end | **Move to the opening**. They've read the submission — lead with the 3 design philosophies, then show them how the screens embody those choices |
| Korean/English mix in the closing | Entire delivery in English |
| Monologue | Expect interruptions — pause after each major section to invite questions |

### 2.1 Recommended In-Person Flow (10-15 min)

**Opening (1-2 min) — Frame the approach**
Lead with the 3 design philosophies (from background.md §1.3):
1. Structured I/O — Consistency by Construction (JSON schemas as prerequisite for measurement)
2. Context Management — Decompose, Then Parallelize (focused stages, cheaper models)
3. Evaluation — Control the Variables (deterministic vs LLM-as-judge separated)

"Before I show you the screens, here's the shape of the thinking. Then each screen will make more sense as an instance of these three choices."

**Demo (6-8 min) — Walk the screens**
Same order as the Loom: Generate → Compare → Metrics → Results/Evidence → Simulation → Template/Transcript.
Each screen: 30-60 sec on *what*, then *which philosophy it expresses*.

**Closing (1-2 min) — Honest gaps**
Pick 2 top limitations from background.md §1.5 and surface them proactively:
- Empirical validation of decomposition (the biggest unvalidated claim)
- Clinical HITL necessity — current eval is purely LLM-driven

"If I had another two weeks, here's exactly what I'd tackle next." — shows self-awareness without defensiveness.

---

## 3. Topics NOT in the Loom (Likely to Come Up)

The Loom covers the product. The interview will push on the *decisions behind* the product. Prepare:

### 3.1 "Why 9 stages instead of one monolithic LLM call?"
Already practiced in Session 03 (Salinna/James). Core answer: experiential — consistent pattern across Gemini/GPT/Claude that models are sensitive to surrounding context. Decomposition lets you retry failed stages, swap models per stage, and optimize each independently.

### 3.2 "How would you empirically validate the decomposition claim?"
From background.md §1.5 (biggest gap):
- N ≥ 20 fixed transcripts/templates
- 3-way comparison: (a) current 9-stage Sonnet pipeline, (b) single-call Opus with full context, (c) single-call Sonnet monolithic
- Measure: per-dimension scores, token cost, wall-clock latency
- One week timeline feasible

### 3.3 "How does clindiff relate to Audit Agent?"
Critical framing (background.md §1.6):
- clindiff = Scribe QA (transcript → note fidelity evaluation)
- Audit Agent = compliance judgment (note vs regulation)
- They are **different problems**. clindiff is not an Audit prototype.
- This opens the door to the Audit KG pitch (§1.8)

### 3.4 "What would you do differently now?"
Pick 2-3 from background.md §1.5 limitations:
- Empirical validation of decomposition
- Multi-Judge Ensemble (single-judge self-preference bias)
- Clinical HITL for ground truth

### 3.5 "Why k3s self-hosted instead of a cloud provider?"
Private take-home, security-conscious stack, full control. Hardening is a prerequisite before any public deployment (background.md §1.5 infrastructure debt).

### 3.6 Jayden's follow-up from Session 03 (unanswered)
James's open question: "If I handed you a week to properly validate the DAG's retry logic, what does the test look like?"
- Same methodology as §3.2 but for retry: inject transient failures at each of 9 stages, measure recovery
- Define "retry success": output identical to no-failure run within tolerance
- Cost ceiling to prevent runaway retries

---

## 4. Practice Method

### 4.1 Recommended Approach
Roleplay walkthrough with interruptions. Jayden narrates the walkthrough; AI plays James (Imperial AI PhD, Scribe Agent owner) and interrupts mid-flow with realistic pushback.

James will push on:
- Empirical rigor (not hand-waving)
- Citation of specific techniques/papers where relevant
- Methodological choices (why this eval approach vs alternatives)
- Whether SNOMED/Clinical Ontology ideas are serious or decorative

Salinna may chime in with softer questions about engineering culture or tradeoffs.

### 4.2 Warm-up Option
Before full walkthrough practice, do a "philosophy-only" version: 2-3 min delivery of just the opening framing, to make sure the 3 philosophies come out cleanly without notes.

### 4.3 Feedback Mode
After each walkthrough practice, request feedback mode from the AI:
- Clarity of the philosophy framing
- Whether key decisions were justified or glossed over
- Whether responses to pushback landed or felt defensive
- English pacing and filler word count

---

## 5. Reference Files

- **Existing Q&A bank**: `/Users/jayden/Library/CloudStorage/GoogleDrive-jayden.chmod@gmail.com/My Drive/job_search/career-assistant/_tracking/motics/interview_qa_all.md` — 40+ drafted answers across tiers T1-T5. Useful for Block 2 (STAR) and Block 3 (role alignment).
- **Full interview context**: `background.md` in this folder
- **Previous practice transcripts**: `2026-04-04-session-01.md`, `2026-04-11-session-02.md` (intern roleplay), `2026-04-20-session-03.md` (Harvinder 1:1 + Salinna/James opener)

---

## 6. Practice Priority Reminder

From background.md §3.2:
1. **clindiff walkthrough** (this doc) — most exposed, needs live narration practice
2. **STAR behavioural stories** — biggest prep gap; draft missing T1-T3 stories
3. **Role expectations / Audit KG pitch** — already well-prepped
4. **Jayden's questions** — expand existing list to 3-4 sharp questions
