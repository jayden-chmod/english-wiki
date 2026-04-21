# Round 3 Q&A — Blocks 2, 3, 4 (Priority Ordered)

Companion to `clindiff-walkthrough-qa.md` (Block 1). This file covers the remaining three blocks of the in-person 3rd round interview:

- **Block 2** — Situational / Behavioural (STAR format) · 15-20 min
- **Block 3** — Role expectations & alignment · 10-15 min
- **Block 4** — Questions Jayden asks back · 10 min

Tier scale is consistent with the existing Q&A bank:
**T1** 거의 확실 · **T2** 높은 확률 · **T3** 가능성 있음 · **T4** 팔로업/심화 · **T5** 낮은 확률

Existing draft locations (Google Drive):
- `interview_qa_general.md` — general + behavioural + project deep dives (Q1, Q3, Q8, Q11, Q12, Q22, Q23, Q24, Q33, Q34, Q37, Q38, + challenge Q)
- `interview_qa_motics.md` — Motics-specific (Q4, Q5, Q6, Q29, Q30, Q31, Q32, Q36)
- `interview_qa_korea.md` — SSG experience deep dives (Q18, Q19, Q20, Q21, Q39, Q40, Q41)

Workflow: Jayden writes draft under each `### Answer` block → AI returns utterance feedback (grammar, articles, register, naturalness, content sharpness).

---

# 📌 BLOCK 2 — Situational & Behavioural (STAR)

**Format reality**: Motics has explicitly confirmed behavioural questions. Expect 4-6 STAR questions covering ownership, failure, conflict, ambiguity, and delivery. The AI person + Salinna likely lead; Harvinder may probe mission/patient-safety angles.

**Biggest prep gap in the round** — per practice-priority ranking, STAR is *least practiced and highest risk* going in.

## STAR Story Bank (pre-load these)

Have **5 well-rehearsed stories** ready — each answers multiple question patterns. Prefer stories already drafted in existing Q&A bank; only build new ones if a gap is identified.

| # | Story | Existing draft | Answers which question types |
|---|-------|---------------|------------------------------|
| S1 | **AutoStore expiry fix** (multi-objective optimisation) | `interview_qa_korea.md` Q21, Q40; `interview_qa_general.md` Challenge Q + Q24 | Ownership · problem redefinition · stakeholder alignment · trade-off design |
| S2 | **Station saturation** (WMS picking delays) | `interview_qa_korea.md` Q20 | Ownership · legacy code · root-cause analysis · floor-user collaboration |
| S3 | **VRO rule-based vs ML choice** | `interview_qa_korea.md` Q18; `interview_qa_general.md` Q12, Q23 | Ambiguity · trade-off · pushback on stakeholder request · cross-functional comms |
| S4 | **React migration + lecture series** | `interview_qa_korea.md` Q39, Q41; `interview_qa_general.md` Q10 | Mentoring · learning-curve leadership · technical choice under uncertainty |
| S5 | **Pitwall / CogEn production difficulty** | `interview_qa_general.md` Q13, Q14, Q16, Q22 | AI-in-production · failure modes · schema validation · safety |

**Stories NOT yet drafted** (build if time permits):
- **S6 — A time you disagreed with a senior person** (see Q11 below — currently 🆕 new)
- **S7 — A time you delivered under hard time pressure** (not explicit in bank — may be needed)

---

## Tier 1 — Almost Certain (T1-T2)

### Q-B1. "Tell me about a failure or mistake you learned the most from."

- **Asker:** Harvinder or Salinna
- **Existing draft:** ✅ `interview_qa_general.md` Q34 — "difficult to answer because of how I handle uncertainty → share early → systemic framing"
- **Status of draft:** Philosophically honest but **lacks a concrete incident**. A STAR answer without a specific Situation is weak. Needs a real story pinned to it — S5 (Pitwall production issues) is the most usable fit.
- **Tier:** T1

#### Answer (draft)
_[fill in — pair the existing framing with a concrete incident]_

---

### Q-B2. "Tell me about a time you owned something end-to-end."

- **Asker:** Salinna (product-ownership model signal)
- **Existing draft:** ✅ Multiple — S1/S2/S3 all work
- **Why it matters:** Motics product-ownership structure — they're evaluating whether you can own an agent solo as a mini-founder. This question essentially asks "show me you've done this before".
- **Recommended story:** **S1 (AutoStore)** — cleanest end-to-end arc (observation → root cause → negotiation → solution → deployment).
- **Tier:** T1

#### Answer (draft)
_[fill in — tighten S1 to STAR shape with explicit Situation/Task/Action/Result]_

---

### Q-B3. "Tell me about a time you disagreed with a colleague or stakeholder — especially someone senior."

- **Asker:** Salinna or Harvinder
- **Existing draft:** 🆕 `interview_qa_general.md` Q11 — status "새로 작성 필요"
- **Why it matters:** They need a peer, not a compliant junior. At 6-person startup there's no shielding manager — you either push back well or produce bad work silently.
- **Candidate stories:**
  - VRO marketing-data pushback (existing Q23) — you told the business team "this cannot work", explained both timing and granularity constraints, agreed to defer
  - AutoStore expiry — initial request was "stop expired items"; you pushed back that the real goal was broader
  - (New) Any SSG case where you challenged a PM/lead's technical choice
- **Tier:** T1

#### Answer (draft)
_[fill in — likely build from Q23 data, reframed as disagreement rather than explanation]_

---

### Q-B4. "How do you handle ambiguity — when requirements aren't clear?"

- **Asker:** Salinna or AI Person
- **Existing draft:** ✅ `interview_qa_general.md` Q12 — VRO / hidden-rules discovery story
- **Status of draft:** Solid, already STAR-shaped. May need slight tightening for verbal delivery.
- **Tier:** T1

#### Answer (draft)
_[fill in — minor polish of Q12 draft]_

---

### Q-B5. "How do you work in a fast-moving startup environment?"

- **Asker:** Salinna or Harvinder
- **Existing draft:** ✅ `interview_qa_general.md` Q8 — "proactive / learn fast / 3-person startup automation example"
- **Status of draft:** Good content, slightly listy — tighten to one-flow narrative.
- **Tier:** T1

#### Answer (draft)
_[fill in]_

---

### Q-B6. "Going from SSG (big company) to a 6-person team is a big shift. What are you excited about, and what worries you?"

- **Asker:** Harvinder
- **Existing draft:** ❌ Not yet drafted
- **Why it matters:** Tests self-awareness about the adjustment. "Nothing worries me" = red flag. "Lots worries me" = red flag. The right answer has one honest concern + how you're already mitigating it.
- **Tier:** T1

#### Answer (draft)
_[fill in]_

---

## Tier 2 — Highly Likely

### Q-B7. "What's your biggest weakness or area for growth?"

- **Asker:** Harvinder or Salinna
- **Existing draft:** ✅ `interview_qa_general.md` Q33 — "Healthcare, I have no prior experience" + career adaptation evidence
- **Status of draft:** Short, clean. The "healthcare" answer is strong because it's true *and* invitingly open — lets them offer an onboarding plan.
- **Tier:** T2

#### Answer (draft)
_[fill in — possibly extend with "what I'm doing about it right now" — reading NICE guidelines, SNOMED etc.]_

---

### Q-B8. "Tell me about a challenge you faced and how you solved it."

- **Asker:** Any
- **Existing draft:** ✅ `interview_qa_general.md` Challenge Q — AutoStore story with first-principles framing
- **Status of draft:** Strong. Already the canonical version of S1.
- **Tier:** T2

#### Answer (draft)
_[fill in — shorten for verbal delivery; current draft is ~350 words which is too long for spoken answer]_

---

### Q-B9. "Tell me about a time you mentored or upskilled someone."

- **Asker:** Salinna (team-culture probe)
- **Existing draft:** 🆕 `interview_qa_general.md` Q10 — marked new
- **Candidate story:** **S4 (React migration + lecture series)** — already partly drafted in Q39. Reshape as mentoring story.
- **Tier:** T2

#### Answer (draft)
_[fill in]_

---

### Q-B10. "How do you handle conflict — with a founder, a peer engineer, or a stakeholder?"

- **Asker:** Harvinder
- **Existing draft:** ❌ Not yet drafted (overlaps with Q-B3 disagreement question but at meta level — "how" not "tell me about a time")
- **Shape of answer:** Your general conflict-resolution principle (share early, separate person from position, anchor on shared goal) + one quick example
- **Tier:** T2

#### Answer (draft)
_[fill in]_

---

### Q-B11. "Walk me through CogEn / Pitwall / your dissertation."

- **Asker:** AI Person (technical backfill)
- **Existing draft:** ✅ `interview_qa_general.md` Q13 (CogEn), Q16 (Pitwall), Q17 (Dissertation), Q14 (belief change)
- **Status of draft:** All drafted. Pick whichever the interviewer asks about; rehearse each to 60-90s.
- **Tier:** T2 — near-certain at least one comes up

#### Answer (draft)
_[fill in — only if existing drafts need revision]_

---

## Tier 3 — Likely

### Q-B12. "If you joined, what do you want to have accomplished 6 months in?"

- **Asker:** Harvinder
- **Overlaps with:** Block 3 Q-R4 (First 90 days). Consider answering this as a longer-horizon version of the same plan.
- **Tier:** T3

#### Answer (draft)
_[fill in — depends on role assignment; draft Audit-Agent version first]_

---

### Q-B13. "Where do you see yourself in 5 years?"

- **Asker:** Harvinder
- **Existing draft:** 🆕 `interview_qa_general.md` Q35 — marked new
- **Shape:** Concrete enough to sound authentic, abstract enough to leave room. Anchor on "product-owner / technical founder" trajectory at intersection of AI and regulated domains.
- **Tier:** T3

#### Answer (draft)
_[fill in]_

---

### Q-B14. "Tell me about a time you delivered under pressure."

- **Asker:** Salinna
- **Existing draft:** ❌ Not yet drafted — this is **S7** candidate
- **Candidate stories:**
  - WMS station saturation during peak — if pressure framing fits
  - Pitwall / clindiff 4-day sprint itself
  - Dissertation milestone under MSc time constraints
- **Tier:** T3

#### Answer (draft)
_[fill in]_

---

### Q-B15. "How do you prioritize when everything seems urgent?"

- **Asker:** Salinna
- **Existing draft:** 🆕 `interview_qa_general.md` Q9 — marked new
- **Tier:** T4

#### Answer (draft)
_[fill in]_

---

### Q-B16. "Why not a bigger, more stable company? You have the CV for it."

- **Asker:** Harvinder
- **Overlaps with:** Q3 (what looking for) + Q5 (why Motics)
- **Tier:** T3

#### Answer (draft)
_[fill in — may merge into Block 3 Q-R1]_

---

### Q-B17. "We might have to work late sometimes. How do you feel about that?"

- **Asker:** Salinna (commitment probe — also a "we're a family" red-flag watch, see `background.md` §6)
- **Tier:** T4
- **Watch:** Don't volunteer heroic "I work any hours". Frame as "can push when it matters, won't build a life around it".

#### Answer (draft)
_[fill in]_

---

# 📌 BLOCK 3 — Role Expectations & Alignment

**Format reality**: 10-15 min block explicitly allocated. Harvinder + Salinna likely lead. This is where:
- The which-agent question gets resolved
- Start date / part-time arrangement gets discussed
- The Audit KG pitch lands if it hasn't already
- Compensation may surface (see `background.md` §5.4)

**Strategy**: Drive the conversation with the role-assignment question early. Don't wait for it.

---

## Tier 1 — Almost Certain

### Q-R1. "Why Motics specifically?" / "Why us?"

- **Asker:** Harvinder (opener for this block, possibly)
- **Existing draft:** ✅ `interview_qa_motics.md` Q5 — "technology should free people → agentic AI → conservative domain → exceptional team"
- **Status of draft:** Good shape. The Salinna speech-enhancement reference is already included.
- **Companion:** `background.md` §5.1 "Why Motics" answer — anchors: MSc methodological fit, stage fit (founding-eng energy), mission fit
- **Tier:** T1

#### Answer (draft)
_[fill in — or polish Q5]_

---

### Q-R2. "Why healthcare?"

- **Asker:** Harvinder
- **Existing draft:** ✅ `interview_qa_motics.md` Q4
- **Practice status:** Already covered in Session 03 — Jayden delivered this well with the "neuro-symbolic fit + Scribe KG opportunity" framing. Rehearse the tightened version.
- **Tier:** T1

#### Answer (draft)
_[fill in if updated delivery needed]_

---

### Q-R3. "Which agent would you see yourself owning?"

- **Asker:** Harvinder or Salinna — **if they don't ask, Jayden must**
- **Existing draft:** ❌ Not yet drafted
- **Shape of answer:**
  - Start with: "I have a ranked preference but want to hear where you see me fitting first"
  - Ranked preference: **Audit > Scribe co-own > Phone Agent**
  - For each, anchor on why:
    - Audit = MSc research fit + SNOMED KG vision
    - Scribe co-own = clindiff was built as Scribe QA + complementary to AI Person
    - Phone = Pitwall/Viola multi-agent background fits
- **References:** `background.md` §2.1.2 (3 scenarios), §5.2 (First 90 Days per scenario)
- **Tier:** T1

#### Answer (draft)
_[fill in]_

---

### Q-R4. "What does the first 90 days look like?" / "First month plan?"

- **Asker:** Salinna
- **Existing draft:** ✅ `background.md` §5.2 — three versions: Audit / Scribe co-own / Phone Agent. Plus an "if undecided" version.
- **Status:** Strong conceptual shape, needs verbal polish. Pick the version matching the answer to Q-R3.
- **Tier:** T1

#### Answer (draft)
_[fill in — pick one version, rehearse]_

---

### Q-R5. "Your MSc runs until September. How do you see the part-time arrangement working?"

- **Asker:** Salinna (logistics framing)
- **Existing draft:** ❌ Not yet drafted (background.md §4.4 lists the question; no answer drafted)
- **Shape of answer:**
  - Concrete availability: **X days/week from May, full-time Sep 1**
  - MSc is complementary, not competing — dissertation feeds directly into Audit work
  - Explicit framing: "I'd want the full-time conversion terms locked in the offer letter so we don't renegotiate in September"
- **References:** `background.md` §5.4 compensation conversation
- **Tier:** T1

#### Answer (draft)
_[fill in — include specific days/hours commitment]_

---

### Q-R6. "What would you improve about our product, if you joined?"

- **Asker:** Salinna or Harvinder
- **Existing draft:** ✅ `interview_qa_motics.md` Q6 — covers Scribe KG + Audit compliance ontology
- **Status:** Solid. Naturally sets up the Audit KG pitch if it hasn't landed yet.
- **Tier:** T1

#### Answer (draft)
_[fill in if update needed]_

---

## Tier 2 — Highly Likely

### Q-R7. "Your dissertation is on Children's Social Care matching with Kairo. Any conflict with Motics work?"

- **Asker:** Harvinder or Salinna (IP / conflict concern)
- **Existing draft:** ❌ Not yet drafted
- **Key angles:**
  - Different domain (social care ≠ medical), different partner (Kairo)
  - Methodology (KG + explainable matching) directly transfers — this is value to Motics, not leakage
  - IP: dissertation IP assignment is between Jayden and UCL + Kairo; not a conflict
  - Practical: Kairo work happens within MSc hours, Motics work in separate time — separable commitments
- **Tier:** T2

#### Answer (draft)
_[fill in]_

---

### Q-R8. "Graduate Visa is 2 years — what's your longer-term plan in the UK?"

- **Asker:** Harvinder (commitment signal)
- **Existing draft:** ❌ Not yet drafted
- **Key angles:**
  - Graduate Visa: 2 years (or 3 for PhD) post-MSc = through mid-2028
  - Sponsorship: Motics being a sponsor for Skilled Worker visa after Graduate Visa ends is the natural path
  - Long-term: London-based, wants to stay in UK healthcare AI
- **Tier:** T2

#### Answer (draft)
_[fill in]_

---

### Q-R9. "If we made you an offer today, what's your decision timeline?"

- **Asker:** Harvinder (close signal)
- **Existing draft:** ❌ Not yet drafted
- **Shape:**
  - Short timeline (3-7 days) if no other active final-rounds
  - Frame as "want to be thoughtful about the offer letter terms, not the decision itself"
  - Leave room for: equity details, part-time→full-time clause, equity acceleration on acquisition
- **Tier:** T2

#### Answer (draft)
_[fill in]_

---

### Q-R10. "What do you know about our competitors? How is Motics different?"

- **Asker:** Harvinder (market awareness)
- **Existing draft:** 🆕 `interview_qa_motics.md` Q7 — marked new
- **Anchor**: Tortus, Heidi, Corti (Scribe space). Motics differentiation: multi-agent suite, NHS pathway orientation, UK-first.
- **Tier:** T2

#### Answer (draft)
_[fill in]_

---

### Q-R11. "Most interesting technology trend in healthcare AI?"

- **Asker:** Harvinder or AI Person
- **Existing draft:** ✅ `interview_qa_motics.md` Q30 — "AI as actor not advisor" framing
- **Status:** Strong conceptual shape.
- **Tier:** T2

#### Answer (draft)
_[fill in if polish needed]_

---

## Tier 3 — Possible

### Q-R12. "How would you approach building a new AI agent for Motics?"

- **Asker:** AI Person
- **Existing draft:** 🆕 `interview_qa_motics.md` Q29 — marked new
- **Tier:** T3

#### Answer (draft)
_[fill in]_

---

### Q-R13. "How would you ensure reliability when an AI agent handles patient data?"

- **Asker:** Harvinder or Salinna
- **Existing draft:** 🆕 `interview_qa_motics.md` Q31 — marked new
- **Tier:** T3

#### Answer (draft)
_[fill in]_

---

### Q-R14. "How do you think about healthcare compliance (GDPR, NHS)?"

- **Asker:** Harvinder
- **Existing draft:** 🆕 `interview_qa_motics.md` Q32 — marked new
- **Tier:** T5 per existing index, but given audit-focus shift, bump to **T3** for round 3
- **Tier:** T3

#### Answer (draft)
_[fill in]_

---

### Q-R15. "What are you looking for in your next role?"

- **Asker:** Harvinder
- **Existing draft:** ✅ `interview_qa_general.md` Q3 — "harder problems / strong team / Motics fits both"
- **Tier:** T3

#### Answer (draft)
_[fill in if needed]_

---

# 📌 BLOCK 4 — Questions Jayden Asks Back

**Format reality**: 10 min explicitly allocated. Must prepare **3-4 sharp questions per person likely in the room**. Asking good questions signals evaluating them, not hoping to be chosen.

**Priority ask order** (if time is limited):
1. **Role-assignment question** (early, probably to Harvinder or Salinna) — unblocks everything else
2. **SNOMED / Audit KG question** — if Audit pitch hasn't landed yet
3. **Runway / Series A question** — to anyone, mutual due diligence signal

Existing draft: ✅ `interview_qa_motics.md` Q36 — only 2 questions drafted (data accumulation, own-model vs API). Needs substantial expansion.

Full question bank from `background.md` §4.5 below, ranked per person.

---

## To Harvinder (CEO — medical doctor, mission-focused)

### Q-A1 (T1 — **ask first if Harvinder opens this block**): Role assignment + Audit KG opener

> "If I joined, which agent would you see me owning? I know I submitted clindiff which reads as Scribe QA, but I also raised the KG angle in round 1 and I've developed the Audit version of that thinking."

**Why first**: Unblocks everything. Answer tells Jayden which pitch to land next.

---

### Q-A2 (T1): SNOMED / regulatory ontology

> "Has Motics looked at SNOMED CT or any regulatory ontology approach for Audit Agent? I've been thinking about how to make Audit auditable in a DCB0129-defensible way."

**Why**: This is the Audit KG pitch opener (`background.md` §1.8) disguised as a question. Use if the role-assignment conversation opens the Audit door.

---

### Q-A3 (T2): Regulatory pathway

> "Where is Motics on the MHRA Class I and DTAC pathway in the next 6-12 months?"

**Why**: Signals you know the regulatory stages. Answer reveals seriousness of healthcare ambition.

---

### Q-A4 (T2): Competitive moat

> "How do you think about Tortus or Heidi expanding into private clinics? What's our moat?"

---

### Q-A5 (T3): Current product traction

> "Of the 5 agents, where is the most customer pull right now? Where's the biggest gap between promise and delivery?"

---

### Q-A6 (T3): Founder's biggest risk

> "What's the biggest risk to the company you lose sleep over?"

**Why**: Founder-level rapport question. Answer also serves due-diligence signal.

---

### Q-A7 (T3): Safety/speed tension

> "How do you balance clinical safety rigour against the move-fast startup reality?"

---

## To Salinna (CTO — already supportive after round 2)

### Q-A8 (T1): Decision-making structure

> "How do technical decisions get made today? Who owns what?"

**Why**: Confirms product-ownership claim. Red flag if answer is vague.

---

### Q-A9 (T1): First two weeks

> "If I joined in May part-time, what would the first two weeks look like?"

---

### Q-A10 (T2): Engineering culture

> "What's the code review / shipping culture like?"

---

### Q-A11 (T2): Tech debt on day one

> "Biggest piece of tech debt you'd hand to a new senior hire on day one?"

---

## To the AI Person (Imperial AI PhD — may own Audit currently)

### Q-A12 (T1 — **must ask, regardless of walkthrough content**): Which agent they own

> "Which agent do you own right now? Walk me through a week in your life on it."

**Why**: Critical unknown (`background.md` §2.1.2). Answer directly determines whether the Audit KG pitch is aligned or politically risky.

---

### Q-A13 (T1): Evaluation methodology

> "How do you currently evaluate your agent's outputs? Human eval, LLM-as-judge, both?"

**Why**: Builds a bridge to clindiff. Signals eval-infra is your value-add.

---

### Q-A14 (T1): SNOMED signal

> "Have you looked at SNOMED CT for any of your work? Curious what your view is on symbolic approaches vs LLM-only in this domain."

**Why**: Tests whether your Audit KG vision conflicts or aligns with theirs. Critical read before committing.

---

### Q-A15 (T2): Product interface

> "If Audit ends up being my territory, where would our products need to interface? What should I expect from Scribe's output, and what should Audit feed back?"

---

### Q-A16 (T2): Eval technique swap

> "I used LLM-as-judge in clindiff for L4-L7. What do you think about G-Eval or pairwise comparison as alternatives? Curious what you've tried."

---

### Q-A17 (T3): Shared infra

> "Where do product owners share code and infra here vs keep things isolated?"

---

### Q-A18 (T3): Their backlog frustration

> "What's something in your current agent you wish you had time to do properly but can't?"

**Why**: Peer-level rapport question. Answer tells you what the team actually struggles with.

---

## To Sales (if present)

### Q-A19 (T2): Customer objections

> "What's the most common objection from clinic directors, and what's our best answer?"

---

### Q-A20 (T2): Demo failure mode

> "When a demo goes wrong, what's usually the failure mode — the product, the pitch, or the fit?"

---

### Q-A21 (T3): Engineer involvement

> "How involved do engineers get in customer calls here?"

---

### Q-A22 (T3): Shipping backlog

> "What's one feature you've been asking engineering for that hasn't shipped yet?"

---

## To Marketing (if present)

### Q-A23 (T3): Lead gen

> "What's working best for lead gen right now — content, events, outbound?"

---

### Q-A24 (T3): Technical content

> "Do you want engineers producing technical content, or is that strictly your turf?"

---

### Q-A25 (T3): Positioning

> "How does Motics position against Tortus and Heidi in actual conversations?"

---

## To Anyone (financials / due diligence)

### Q-A26 (T1 — **must ask at least one of these**): Series A timeline

> "Could you share where you are on the Series A timeline? I'd like to understand the runway context."

**Why**: Mutual due diligence is explicit per round-3 format. Not asking = missed signal.

---

### Q-A27 (T2): Investor conversations

> "Morgan Stanley MSISV Demo Day was February — has that opened up new investor conversations?"

---

### Q-A28 (T2): Current traction numbers

> "Current paying clinic count and month-over-month growth — is that something you can share?"

---

# 📌 Practice Strategy

## Order of drafting (if time-boxed)

1. **Block 2 Q-B1–B6** — the 6 Tier-1 STAR questions. Every one of these is almost certain.
2. **Block 3 Q-R3, Q-R4, Q-R5** — which-agent, first-90-days, part-time arrangement. These drive the block.
3. **Block 4 Q-A1, Q-A12, Q-A26** — the "must ask" trio: role assignment, AI Person agent ownership, runway.
4. Then fill in Tier 2 questions in both Block 2 and Block 3.
5. Tier 3+ only if prep time allows.

## Story reuse discipline

- Don't use the **same story twice** in the interview. If S1 (AutoStore) is used for Q-B2 (ownership), use S2/S3 for Q-B3 (disagreement).
- Have **S6 (senior disagreement)** ready as a separate story — this is the weakest slot in the current bank.

## English pacing targets

- STAR answer: **90-120 seconds spoken** (longer than Block 1 answers — behavioural needs narrative arc)
- Role-alignment answer: **60-90 seconds**
- Question back to interviewers: **15-25 seconds** (be short; let them answer at length)

## Wiki cross-refs to watch for in drafts

- [[missing-articles]] — high-severity pattern; STAR answers are article-heavy ("the manager", "the system", "a bin")
- [[informal-register-in-interview]] — avoid `gonna`, `kinda`, `guys`, `y'know`
- [[subject-verb-agreement-errors]] — watch "data are/is", "the team has/have"
- [[take-you-to]] — if any answer involves UI navigation, use `takes` not `navigates`
