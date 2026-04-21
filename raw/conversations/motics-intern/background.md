# Motics 3rd Round Interview — Roleplay Context

## How to Use This Document

This is a focused context document for roleplay practice ahead of the Motics 3rd round interview. Background on the company, founders, and my profile is already loaded in the tool. This document adds:

1. The 2nd round take-home submission (clindiff) — likely to be discussed deeply
2. **My separate Audit KG + SNOMED CT vision** — the core pitch for round 3 (partially raised in round 1, needs completion)
3. The 3rd round scenario and how to roleplay it
4. Targeted background only where needed for specific questions

The AI playing the interviewer should roleplay as one of: **Harvinder (CEO)**, **Salinna (Co-founder, technical lead)**, **the AI Person (Imperial AI PhD)**, or **Sales / Marketing team members**.

**Key strategic state going into round 3**:
- Round 1: I raised the KG/Ontology angle applied to Scribe Agent. Audit angle prepped but not presented due to timing.
- Round 2: Submitted clindiff — LLM-as-judge eval platform, best suited for Scribe QA.
- Round 3 goal: Complete the vision by landing the Audit KG + SNOMED CT pitch, confirm which agent I'd own, and lock chemistry with the full team.

---

## 1. The 2nd Round Submission: clindiff

### 1.1 What It Is
**clindiff** is a clinical-note evaluation platform built during a 4-day sprint. It compares outputs from multiple LLMs against the same transcript, evaluating quality, structure adherence, faithfulness, and reproducibility.

**Positioning**: Not a "finished product" — a starting point. The Limitations section reads like a roadmap document, not a list of excuses.

### 1.2 Architecture: 9-Stage DAG Evaluation Pipeline

```
L0: Preprocessing (Claude Sonnet 4.6, Internal Parallel)
 ├── L1: Generation
 └── L2: Evidence Extraction (Claude Sonnet 4.6)
      │
      └──> L3: Structure Validation (Deterministic)
             │
             ├── L4: Compliance ──┐
             ├── L5: Faithfulness ├──> L7: Clinical Judge
             └── L6: Completeness ┘
                    │
                    └──> L8: Comparative / Nuance
```

**Execution model**: 3 batches running in parallel. Once all complete, a new note generation step triggers, followed by parallel evaluation (L4-L8) using Claude Sonnet 4.6 to ensure safety and rationale.

### 1.3 Three Design Philosophies

1. **Structured I/O — Consistency by Construction**: JSON schemas as a mandatory prerequisite for objective, atomic measurement. Prose is for humans; JSON is what models and validators consume.

2. **Context Management — Decompose, Then Parallelize**: Instead of one monolithic LLM call, break the evaluation into focused, specialized stages. Each stage gets only the context it needs, enabling smaller/cheaper models per stage.

3. **Evaluation — Control the Variables**: Separate deterministic checks (Structure) from LLM-as-judge signals (Compliance/Faithfulness/Completeness/Safety) so we can isolate where variance comes from.

### 1.4 Tech Stack
- **Frontend**: Next.js 16.2.3 + React 19.2.4 + Tailwind v4 + TypeScript
- **UI components**: shadcn/ui + Base UI, Zustand + Zod, Monaco editor
- **Backend**: FastAPI + Python 3.13, SQLAlchemy 2.0 (async) + asyncpg, PostgreSQL 16
- **LLM SDKs**: Anthropic SDK, Google google-genai
- **Deployment**: k3s self-hosted on Oracle Cloud, Traefik Ingress (ports 80/443)
- **Testing**: Vitest + Playwright
- **CI/CD**: Intentionally skipped — manual deploy workflow chosen for private-server security

### 1.5 Limitations I Explicitly Called Out

**Technical & Architectural**:
- **Compiler Hallucination & Missing HITL**: The Level 0 Template Compiler relies on an LLM to generate schemas with no mandatory Human-in-the-Loop approval gate. UI exists for manual editing but isn't mandatory yet.
- **Schema Granularity vs Generalization Trade-off**: Table parsing case study — highly specific rules would improve extraction but cause overfitting. Current generalized approach struggles with complex tables.
- **Prose → JSON Rule Object Compilation** (central insight): Evaluative qualifiers like "include relevant past medical history" or "summarize concisely" resolve inconsistently across models. This is the primary source of cross-model variance in the Simulation view. The fix: a Template Compiler that translates prose rules into a structured JSON rule object — preserving human-authored phrasing while making constraints machine-checkable.
- **Partial AST for Schema Extraction**: AST parsing is only at section level. Extending to field-rule level is the natural next step and converges with the rule-compilation work above.
- **Limits of Semantic Validation**: Level 3 is strictly structural. Can't verify if "Plan" content is mistakenly placed in "Assessment". Next step: Clinical Ontology layer.
- **Unverified Retry Logic across 9 Stages**: DAG supports re-executing individual nodes but not fully stress-tested. Automated retry loops omitted intentionally to prevent API cost spikes.
- **High Latency (>1 min)**: Current mitigation is content-addressable hashing for Template Analysis, Transcript Preprocessing, and L2 Evidence Extraction. Future: Map-Reduce pattern with semantic chunking.
- **Empirical Validation of Decomposition Gains** (the biggest gap): The "focused context + smaller model" hypothesis is adopted as a design principle, but not measured against a monolithic baseline. Proposed benchmark: on N ≥ 20 fixed transcripts/templates, compare (a) current 9-stage Sonnet pipeline, (b) single-call Opus with full template + transcript, (c) single-call Sonnet with the same monolithic prompt. Measure per-dimension scores, token cost, wall-clock latency.

**Evaluation & Validation**:
- **Lack of Human Pipeline Audit**: The 9-stage evaluation logic hasn't been manually verified against human judgment.
- **Clinical HITL Necessity**: Current evaluation is purely LLM-driven — risks an "echo chamber" of model preferences. True validation needs clinical ground truth from medical professionals.
- **LLM-as-Judge Bias**: A single judge exhibits self-preference bias — scores outputs from its own model family higher. Mitigation: Multi-Judge Ensemble (Claude + Gemini in parallel), with inter-judge agreement exposed as a first-class metric. The spread between judges is signal, not noise — it localizes which dimensions are genuinely ambiguous vs stylistically contested.
- **Quantifying the bias itself**: Measure the average score each judge assigns to outputs from its own family vs others — makes self-preference observable, not a vague concern.

**Strategic Alignment Note**: This is an Evaluation System per the assignment. I prioritized reliability of the 9-stage rubric over the final generation's prose quality. Structured JSON was a prerequisite for objective, atomic measurement. Once the evaluation logic is finalized, it serves as a deterministic reward model for refining the generation engine.

**Infrastructure Debt**:
- **No Prompt Injection Defenses**: Intentional scope omission — private take-home, known user base, negligible attack surface. Hardening is a prerequisite before any public or clinical deployment.
- **SSE Buffering via Cloudflare**: Works in local dev but buffered in production. 9KB dummy padding implemented, real-time delivery still unstable.
- **Rapid Prototyping Debt (4-Day Sprint)**: Code quality sacrificed for functional integration. Lacks high-level abstraction and automated test coverage.

### 1.6 How clindiff Relates to Motics' Products

**Important clarification on what clindiff is and isn't**:

clindiff is an **LLM-as-judge evaluation platform** for the transcript→note conversion problem. This most directly maps onto **Scribe Agent QA** — evaluating whether the Scribe Agent's generated note faithfully reflects the transcript.

It is **not** a prototype of Audit Agent. Audit Agent is a different beast — it checks clinical notes against UK regulatory requirements (HCPC, GMC, CQC, GDPR, NHS Digital), which is a **compliance judgment problem**, not a fidelity evaluation problem.

The three agents relate like this:
```
[Consultation] 
    ↓
[Scribe Agent] ──→ [Clinical Note] ──→ [Audit Agent] ──→ [Compliance Report]
                          ↑
                   clindiff evaluates
                   the quality of this step
```

So clindiff is best understood as:
- A **general LLM-evaluation toolkit** that could harden the Scribe Agent's QA
- A **capability proof** showing I can design evaluation systems end-to-end
- **Independent from** my separate vision for Audit Agent (see Section 1.7)

Framing clindiff as "Audit Agent prototype" would be both technically inaccurate and politically risky — it would imply I'm encroaching on whoever owns Audit. The correct framing is: clindiff strengthens Scribe's reliability, Audit has its own distinct approach.

### 1.7 My Separate Vision for Audit Agent: KG + Ontology Approach

This is **the core pitch I need to land in round 3**. I raised the Scribe + KG angle in round 1 but ran out of time to present my Audit thinking. Round 3 is the time to complete the vision.

**The problem with LLM-only Audit**:
Regulations are fundamentally **structured rule sets**. Using an LLM to interpret them produces:
- Inconsistent judgments (different runs, different verdicts on the same note)
- Opaque reasoning (no clean audit trail for "why this failed compliance")
- Context window limits (can't cram the full HCPC/GMC/CQC/GDPR corpus into every prompt)
- Poor fit with DCB0129/DTAC clinical safety case requirements (which demand traceable decision logic)

**The KG-augmented approach**:
```
[Clinical Note]
      ↓
[LLM Extraction] ──→ entities: "patient has type 2 diabetes,
                               prescribed metformin, HbA1c 8.2%"
      ↓
[SNOMED CT Mapping]
  - "type 2 diabetes" → 44054006 |Type 2 diabetes mellitus|
  - "metformin"       → 109081006 |Metformin|
  - "HbA1c"           → 43396009  |Hemoglobin A1c measurement|
      ↓
[Structured Representation in OWL]
  Patient ⊑ ∃hasCondition.|Type 2 diabetes mellitus|
  Patient ⊑ ∃prescribedMedication.|Metformin|
      ↓
[Regulatory KG Query — Description Logic / SPARQL]
  - GMC Good Medical Practice §15: documentation completeness
  - NICE NG28 (T2DM guideline): first-line treatment check
  - GDPR Article 9: sensitive data handling
      ↓
[Compliance Judgment WITH Full Explanation Path]
  ✓ Diagnosis documented with SNOMED code: 44054006
  ✓ Medication matches NICE NG28 first-line recommendation
  ⚠ HbA1c documented but no follow-up interval specified
     (violates NG28 §1.6.14)
```

**Why SNOMED CT specifically**:
- **NHS mandated** terminology for primary care since 2020 — not optional if Motics wants NHS market
- **OWL 2 EL based** — which is exactly the Description Logic profile I studied in UCL MSc
- **Maintained by NHS Digital** with UK Edition — high quality, regularly updated
- **35K+ concepts, 1M+ relationships** in hierarchical subsumption structure
- **DTAC interoperability scoring** explicitly checks for SNOMED support
- **FHIR UK Core** (likely used by Motics' EHR integrations) treats SNOMED as default terminology

**The Neuro-Symbolic split**:
- **LLM does what it's good at**: extracting entities from unstructured clinical prose
- **KG does what it's good at**: deterministic, auditable, provably sound rule application
- Together: the best of both paradigms — flexible input understanding + reliable compliance judgment

**Direct connection to my MSc dissertation**:
> "Extracting structured knowledge from unstructured case data, building a Knowledge Graph, and delivering explainable matching aligned with UK Algorithmic Transparency requirements."

Only the domain differs (Children's Social Care → UK Medical Regulations). Methodology is identical. This means the MSc research directly produces IP that Motics can deploy.

### 1.8 Pitch Script for Round 3

**When to deploy this pitch**:
- If Harvinder asks "what would you want to work on?"
- If the conversation naturally turns to Audit Agent
- In Q&A at the end if no natural opening came up (but only if time permits)

**The script (memorize the shape, not the exact words)**:

> "In round 1 I mentioned the KG/Ontology angle for Scribe, but I think it actually fits Audit even better. I've been looking at the UK medical regulatory landscape, and a lot of it is already formalized — SNOMED CT is NHS-mandated, it's OWL 2 EL based, and NHS Digital maintains the UK Edition. So Audit doesn't need to be a build-an-ontology-from-scratch project. The approach would be: use an LLM to extract clinical entities from notes, map them to SNOMED CT, then express regulatory rules as Description Logic queries over that representation. The match path itself becomes the audit trail — which is exactly what DCB0129 and DTAC clinical safety cases require. This is also the same methodology as my MSc dissertation — I'm doing this for Children's Social Care, so the research directly transfers. Has Motics looked at SNOMED CT or any regulatory ontology approach for Audit?"

**What this accomplishes in one breath**:
1. **Continuity with round 1** — picks up unfinished thread
2. **Concrete technical references** — SNOMED CT, OWL 2 EL, DL, DCB0129, DTAC (signals I've actually done the reading)
3. **Executable architecture** — not hand-waving
4. **MSc connection** — research time = company value, not a conflict
5. **Business justification** — NHS market entry requires this anyway
6. **Ends with a question** — invitation to dialogue, not monologue

**The answer to that final question tells me a lot**:
- "Yes, we've looked at SNOMED" → my approach aligns; I can contribute day one
- "No, we haven't gotten there yet" → space to add value, but find out why not
- "We considered but chose LLM-only" → need to defend my position with specifics

### 1.9 Anticipated Pushback on the Audit KG Pitch

**Pushback 1**: "SNOMED CT is huge and complex. Can you actually handle 350K concepts?"

Response: "Don't need the whole thing. Motics-relevant subsets — primary care, physiotherapy, common conditions — are maybe 50-100K concepts. NHS Digital publishes specialty-specific reference sets. Reasoners like HermiT or ELK handle EL profile efficiently. We'd load only the slices we need."

**Pushback 2**: "If LLM extraction is noisy, doesn't the whole system collapse?"

Response: "That's why extraction needs multi-stage validation. Confidence scores on each SNOMED mapping, low-confidence cases route to human review. And clindiff's L5 Faithfulness evaluation applies directly here — we check that the LLM didn't extract content that isn't actually in the note. That's the point where clindiff and Audit KG connect, even though they're separate systems."

**Pushback 3**: "If the mapping step still uses an LLM, how is this more deterministic than LLM-only?"

Response: "Two differences matter. One, once a concept is normalized to a SNOMED ID, everything downstream is deterministic — same code, same meaning, forever. Two, mapping failures are *explicit* — 'this term does not map to SNOMED' is a clean drop. Pure LLMs don't know when they're wrong, they just generate confidently. In a regulated domain, that difference is huge."

**Pushback 4**: "Why not just stuff SNOMED CT into the LLM context and prompt for compliance?"

Response: "Two hard limits. Context window — 350K concepts don't fit. Consistency — same note, multiple runs, different answers. DL reasoners are mathematically sound, so same input guarantees same output. For a regulatory audit product, that reproducibility is the value."

**Pushback 5** (the trickiest): "This sounds like overengineering. We're a 6-person startup, not a research lab."

Response: "Fair concern. My answer: this is the approach that gets us NHS-ready and DCB0129-defensible. The alternative — LLM-only Audit — saves time short-term but hits a wall when NHS or CQC ask for the audit trail. Better to build it right from the start when we're small, than retrofit explainability later under regulatory pressure. And my MSc research timeline means a lot of this foundation gets built for free before September."

### 1.10 Pre-Round-3 Prep for the Audit Pitch

If I have 15-30 minutes to prep, scan these:

1. **SNOMED CT UK Edition Release Notes** (most recent) — signals I track active changes
2. **One NICE Guideline** (e.g., NG28 diabetes, NG136 hypertension) — so I can give a concrete rule-encoding example when pushed
3. **HL7 FHIR UK Core + SNOMED** — since Motics' EHR integrations (Cliniko, Meddbase) likely touch FHIR
4. **DCB0129 / DCB0160** — clinical safety case structure, to reference convincingly
5. **DTAC criteria** — to map my approach to their rubric

---

## 2. The 3rd Round: What It Is

### 2.1 Known Facts

**Status**: 2nd round passed (confirmed 2026-04-20). 3rd round scheduled.

**Confirmed 3rd round format** (from Motics email):
1. Meet the Motics team in the office (in-person)
2. Brief in-person walkthrough of take-home assessment (clindiff) + Q&A
3. Situational and behavioural questions
4. A conversation around role expectations and alignment
5. Time set aside for questions from Jayden

**What this format tells us**:
- **In-person** — office, not video call. Energy, presence, and body language matter.
- **Clindiff walkthrough is the explicit technical centrepiece** — they've read it, want to hear Jayden walk through it live. Likely 10-15 min overview + targeted Q&A, not a deep-dive from scratch.
- **Behavioural questions are explicitly on the agenda** — STAR format. This is new territory not covered in previous practice. Needs dedicated prep (see Section 4 Expected Questions).
- **"Role expectations and alignment"** is explicitly scheduled — the which-agent question will be asked by them, not just by Jayden. Come prepared with a ranked preference and concrete first-90-days plan.
- **Jayden's questions are explicitly allocated time** — this is unusual and signals they want genuine mutual due diligence. Use it.

**What changed from speculation**:
- The "Harvinder 1:1 as opener" structure we practiced may not happen. The whole team may be present together from the start (see note in 3.2).
- "Why healthcare" is less likely to be the opener — clindiff walkthrough probably comes first.
- Behavioural questions (STAR) are explicitly confirmed — previously assumed but not certain.

- **Signal from 1st round**: Salinna said "we don't run long processes" — strong indicator this is the final round before offer.
- **Signal from 1st round**: They already floated "starting part-time from May" — meaning internal lean toward hiring is strong.
- **2nd round interviewer**: Salinna (Co-founder, technical lead).
- **Team composition (5 current members)**:
  1. **Harvinder Power** — Co-founder & CEO, medical doctor
  2. **Salinna Abdullah** — Co-founder, technical lead
  3. **AI person** (name unknown, Imperial College London AI PhD background) — owns one of the AI products/agents end-to-end
  4. **Sales** (name unknown) — clinic acquisition, commercial
  5. **Marketing** (name unknown) — brand, content, growth
- **How Motics structures work**: Each person owns one product/agent end-to-end. Not a role-based split (backend vs frontend vs ML), but **product ownership**. For example, one person might own Scribe Agent entirely — from model choice to prompts to the UI to the EHR integration.
- **I would be the 6th hire, and likely be assigned ownership of one of the 5 agents** (or a new one — Audit Agent being the most likely, given my clindiff submission).
- **Compensation / terms**: Not yet discussed.

### 2.1.1 What This Team Structure Actually Means

This is a critical nuance I initially misread. This isn't a company with "an AI researcher" and "sales" and "marketing" as specialist roles. It's a company where **each person is a mini-founder of their product line**.

Implications:

- **The "AI person" isn't doing abstract research — they're shipping a product.** Imperial AI PhD background means they bring research rigor to it, but they're also writing code, talking to customers about that agent, and owning its roadmap. They're a full-stack product owner with AI depth.

- **My role, if hired, is probably not "platform engineer serving everyone else."** It's more likely: "You own [some agent]. Go make it work." That's full-stack product ownership — build, evaluate, integrate, iterate, talk to customers.

- **The AI person and I would be peers, not a hierarchy.** Each of us owns our own agent. We'd share model infrastructure and engineering practices but be accountable for our own product's success.

- **Autonomy is very high. Support structure is very thin.** There's no engineering manager. There's no product manager. It's "you own it, you figure it out." This is either exhilarating or terrifying depending on the person — and they're testing which one I am.

- **The "founding engineer" framing is even more accurate now.** This is not hire #6 joining an engineering team. It's hire #6 becoming one of 6 product owners.

### 2.1.2 Current State of Signals (What I Know About Ownership)

Partial, unconfirmed information:
- **1st round**: I raised KG/Ontology angle applied to Scribe Agent. Audit angle was prepped but didn't get time to present.
- **2nd round**: Submitted clindiff — LLM-as-judge eval platform, most directly applicable to Scribe QA.
- **Rumor/hint**: The AI person (Imperial AI PhD) may be building Audit Agent currently. Unconfirmed.
- **Team structure**: Product ownership model. Each person owns an agent end-to-end.

Given these signals, three plausible scenarios for what role I'd fill:

**Scenario A — I own Audit Agent (new or inherited)**
- My round-3 Audit KG pitch lands and they want me to execute it
- If AI person is currently on Audit, there's a handoff dynamic
- Domain knowledge gap: UK medical regulations (HCPC, GMC, CQC, GDPR, NICE)
- MSc dissertation work feeds directly into this
- This is my **preferred outcome** — fits my research and the KG vision

**Scenario B — I co-own Scribe with the AI person, focus on QA/reliability**
- clindiff is the most direct proof of fit here
- AI person keeps modeling and prompting; I take evaluation infra, production hardening, regulatory artifacts
- Lower risk than Audit (no regulatory domain gap) but less aligned with MSc research
- Still a strong outcome if Audit is taken

**Scenario C — I own Phone Agent or platform/infra layer**
- clindiff was capability proof, not product assignment signal
- Phone Agent fits my multi-agent background (Pitwall LangGraph, Viola Docker orchestration)
- Platform/infra fits my FDE background at SSG
- My Audit KG pitch becomes a "future direction I'd contribute toward" rather than my immediate role

**How to figure out which scenario is real, early in round 3**:

Ask naturally (probably to Harvinder or Salinna, not the AI person first):
> "If I joined, which agent would you see me owning? I know I submitted clindiff which reads as Scribe QA, but I also raised the KG angle in round 1 and I've developed the Audit version of that thinking. I'd love to know where you see me fitting."

This single question:
- Signals I understand the product-ownership structure
- Telegraphs that I have an Audit vision (without launching into it uninvited)
- Gives them a reason to open the topic naturally
- Gives me the information I need to choose which pitch to land

**The ideal sequence**:
1. Ask the role-assignment question early
2. If answer points toward Audit or is open, deploy the Audit KG pitch (Section 1.8)
3. If answer points toward Scribe co-ownership, pivot to how clindiff + AI person's work compose
4. If answer points toward Phone/platform, pivot to Pitwall/Viola relevance and frame Audit KG as a "longer-term contribution"

### 2.1.3 How Motics Structures Work (Reminder)

Each person owns one product/agent end-to-end. Not a role-based split (backend vs frontend vs ML), but **product ownership**. One person might own Scribe Agent entirely — from model choice to prompts to the UI to the EHR integration. This means:

- **High autonomy, thin support**: No PM, no EM. "You own it, figure it out."
- **Mini-founder framing**: Hire #6 becomes one of 6 product owners, not an engineer serving other engineers.
- **Equity expectations should reflect ownership**: 1-2% range is reasonable for this model, not 0.5%.
- **Chemistry check is about trust**: Can they trust me to own a product autonomously in a regulated, customer-facing medical space?

### 2.2 What They're Really Evaluating

Given the product-ownership structure, round 3 is answering a specific set of questions:

1. **Can I own a product end-to-end?** Not just code it — but decide scope, talk to customers, evaluate quality, iterate. clindiff showed I can design a system; round 3 tests whether I can carry one.

2. **Will I be a peer to the existing product owners, not a subordinate?** They need someone who can disagree with Salinna on engineering choices, push back on the AI person's model decisions when it affects my product, and say no to Sales when a customer ask is wrong for the roadmap.

3. **Do I handle ambiguity without melting down?** Product ownership with no PM/EM means making decisions with incomplete information constantly. If I look like someone who needs clear tickets, I'm out.

4. **Can Harvinder (CEO) align with me on mission, and trust me to represent Motics to clinicians?** Because I'll be on customer calls owning my product.

5. **Will Sales and Marketing find me useful as a partner**, not just a code-behind-the-curtain engineer?

The decision isn't "is this person technically good enough" — that's already answered. It's "do we want this person as the 6th mini-founder?"

### 2.3 My Objectives for Round 3
- **Confirm they want to hire me** (they likely do) and set up offer conversation
- **Negotiation-ready positioning**: surface that I'm thinking at Founding Engineer level, not just "full-stack dev"
- **Mutual due diligence**: I'm evaluating them as much as they're evaluating me
- **Anchor specific first-project proposal**: "If I join, month 1 I'd extend clindiff onto real Motics data and lay the Audit Agent eval foundation"

---

## 3. Roleplay Scenario Guide

### 3.1 Characters the AI Can Play

**Harvinder Power (Co-founder & CEO)** — medical doctor background, Imperial College London medical school, NHS Clinical Entrepreneur Programme alum. Blends clinical detail with business narrative. Core mantra: "AI expands clinic capacity without scaling cost." Cares deeply about patient safety, regulatory pathway, NHS vs private market strategy. Asks mission-alignment questions. Probes on hallucination risk and clinical safety. In this round, he's checking: can this engineer represent Motics to clinicians? Will they push back on me when I'm wrong about the medicine?

**Salinna Abdullah (Co-founder, technical lead)** — engineer, founder. Already interviewed me in round 2. Tone is warmer, more collaborative. Interested in implementation details, engineering culture, how I'd ship things. Openly talks about the structural pain of B2B HealthTech (12-24 month sales cycles, grant dependency). Values "builders" identity. In this round, she's probably mostly observing — she already made her decision in round 2.

**The AI Person** (name unknown, **Imperial College London AI PhD**) — owns one of Motics' AI products end-to-end as a mini-founder with AI depth. Not a detached researcher. Writes code, tunes prompts, picks models, talks to customers.

**Important unknowns**:
- Which agent do they currently own? (Scribe is most likely given modeling depth needed, but Audit is possible)
- If they're on Audit, my KG pitch is politically sensitive — it implicitly critiques their approach
- If they're on Scribe, my KG pitch is safer — I'm proposing work in a different product

**How to handle regardless of which agent they own**:
- Treat clindiff as a prototype, not a finished product
- Open by asking what they've learned building their agent — don't lecture
- Signal complementary skills: I'm stronger on eval infra, data pipelines, production hardening, regulatory artifacts. They're stronger on modeling, prompting, research rigor.
- If they push back on my KG ideas, engage with the technical substance — don't cave, don't dig in

Given the Imperial AI background, they'll have strong opinions on evaluation methodology and will probe clindiff on: (1) empirical validation of decomposition claim, (2) multi-judge ensemble theoretical basis, (3) whether "Clinical Ontology" and SNOMED CT reasoning is serious or hand-wavy, (4) statistical rigor in evaluation. They'll notice if I cite vague "best practices" vs specific papers or techniques.

Not necessarily territorial — product ownership model means there's no "AI research" territory to defend. But they care deeply about: do I respect methodological rigor, or am I a ship-fast-break-things engineer who'll produce embarrassing bugs on a medical product?

**Note on shared context with Harvinder**: Harvinder is also Imperial (medical school). The AI person being Imperial AI is likely a direct recruit from Harvinder's network — trusted deeply. Don't try to play them against each other.

**Sales Lead** (name unknown) — commercial-facing, has direct conversations with clinic directors and practice managers. Evaluates whether I can demo, explain, and defend the product to a non-technical customer. Often underestimated voice in engineering hires but actually critical — if sales doesn't trust the engineer, feature requests get stuck in translation. Questions will likely sound simpler but have sharp edges: "Why does this take so long?" "What does the clinic see if the AI gets it wrong?" "Can you join a customer call and explain this?"

**Marketing Lead** (name unknown) — brand and narrative. Cares about whether I can produce demos, case studies, technical content that supports commercial motion. May ask about what's shippable as a public artifact or milestone. Also the person most likely to float culture-fit questions casually ("what do you do outside work?").

### 3.2 Confirmed Session Structure (updated 2026-04-20)

The confirmed format maps to these practice blocks:

**Block 1 — clindiff walkthrough (10-15 min)**
Live walk-through of the take-home in front of the team. Not a from-scratch explanation — they've read it. The goal is to narrate the key decisions (why 9 stages, why JSON, why the specific eval split) confidently and concisely, then handle targeted Q&A. Likely the most technically exposed moment.
- Key: lead with the 3 design philosophies, then take questions. Don't over-explain unprompted.
- Who asks: probably the AI person and Salinna. Harvinder may ask about clinical safety implications.

**Block 2 — Situational and behavioural questions (15-20 min)**
STAR-format questions about past experience. Explicitly confirmed by Motics. Questions will probe: ownership, failure, conflict, ambiguity, delivery under pressure.
- Prepare 4-5 STAR stories from SSG and UCL that can be adapted to different questions.
- Key stories to have ready: (1) owned something end-to-end at SSG, (2) a failure and what I learned, (3) disagreed with someone senior, (4) made a decision with incomplete information, (5) delivered under pressure.
- See Section 4.1 for expected questions.

**Block 3 — Role expectations and alignment (10-15 min)**
Which agent, start date, part-time terms, what month 1 looks like. Harvinder and Salinna likely lead this. Come with a ranked preference and a concrete first-90-days proposal (see Section 5.2).
- This is where the Audit KG pitch lands naturally if it hasn't come up in Block 1.
- Compensation may come up — see Section 5.4.

**Block 4 — Jayden's questions (10 min)**
Explicitly allocated. Use it fully. Prepare 3-4 sharp questions (see Section 4.5). Asking good questions signals you're evaluating them, not just hoping to be chosen.

**Practice priority order** (given confirmed format):
1. clindiff walkthrough — most exposed, most practiced by re-reading clindiff and narrating it aloud
2. STAR behavioural stories — least practiced so far, highest risk
3. Role expectations / Audit KG pitch — already well-prepped in Section 1.8
4. Jayden's questions — already listed in Section 4.5

**Note**: The previous "Harvinder 1:1 as opener" structure (practiced in Session 03) is now less likely to be the actual format. The confirmed sequence starts with clindiff, not mission alignment. That said, the Harvinder content (why healthcare, clinical safety, patient harm framing) is still relevant — it will surface in Block 1 or 3 when the full team is present.

### 3.3 Tone Guide

**UK early-stage startup interview style**:
- Smart casual, not suited
- Self-deprecating humor preferred
- Direct technical questions mixed with chemistry check
- "Can we work together?" frame dominates
- NOT the American hard-sell culture

**What I should avoid**:
- Bragging about SSG scale (Kafka throughput, team size)
- Korean tech superiority undertone
- Over-emphasizing UCL student status
- Excessive Korean-style deference
- Over-praising the company

**What I should project**:
- Genuine curiosity
- Humble confidence (the take-home is already proof)
- Hands-on builder identity (I've shipped things)
- "Already one of the team" energy, not applicant mode
- Concrete contribution plan (first milestone proposal)

---

## 4. Expected Question Categories

### 4.1 Culture Fit Questions
- "Going from SSG (a big company) to a 6-person team is a big shift. What are you excited about, and what worries you?"
- "Tell me about a failure or mistake you learned the most from."
- "How do you handle conflict — with a founder, a peer engineer, or a stakeholder?"
- "Where do you want to be in 5 years?"
- "If you joined, what do you want to have accomplished 6 months in?"
- "Why not a bigger, more stable company? You have the CV for it."
- "We might have to work late sometimes. How do you feel about that?"

### 4.2 Technical Questions (clindiff-based)
- "Walk me through why you split it into 9 stages instead of one big LLM call."
- "You mentioned Clinical Ontology as a next step. How would you actually build that?"
- "For the Multi-Judge Ensemble — how would you compute inter-judge agreement as a metric?"
- "The prose-to-JSON rule compiler — sketch the implementation."
- "How would you detect hallucinations in production, not just in eval?"
- "You said decomposition is the central architectural claim but unverified. If you had one week to run that benchmark, what does it look like?"
- "The k3s self-hosted setup — why not AWS/GCP? Production scaling concerns?"

### 4.2.1 Technical Questions (Audit KG / SNOMED pitch follow-ups)
If I land the Audit KG pitch, expect these as follow-ups:
- "Walk me through a concrete example. Pick one regulation, show me how it becomes a DL query."
- "What happens when the LLM extracts an entity that doesn't map cleanly to SNOMED?"
- "How do you handle the ontology's complexity — 350K concepts, deep hierarchies?"
- "What's the dev cycle? Adding a new regulation — how long does that take?"
- "Have you actually written OWL and SPARQL, or is this theoretical?"
- "Which reasoner would you use? HermiT, ELK, Pellet? Why?"
- "How does this interface with Scribe's output? What's the handoff format?"
- "SNOMED CT licensing — have you looked at the cost and restrictions?"

### 4.3 Business / Product Understanding Questions
- "Who do you think our main competitors are? How do we win?"
- "NHS or private clinics — where should we double down first?"
- "Why does MHRA Class I registration matter? Walk me through it."
- "We're pitching to a clinic director tomorrow. What's our strongest argument?"
- "Five agents — which one is most defensible long-term? Why?"

### 4.4 Logistics / Commitment Questions
- "Your MSc runs until September. How do you see the part-time arrangement working?"
- "Your dissertation is on Children's Social Care matching with Kairo. Any conflict with Motics work?"
- "Graduate Visa is 2 years — what's your longer-term plan in the UK?"
- "If we made you an offer today, what's your decision timeline?"

### 4.5 Questions I Should Ask Back

**To Harvinder**:
- "Where is Motics on the MHRA Class I and DTAC pathway in the next 6-12 months?"
- "How do you think about Tortus or Heidi expanding into private clinics? What's our moat?"
- "Of the 5 agents, where is the most customer pull right now? Where's the biggest gap between promise and delivery?"
- "What's the biggest risk to the company you lose sleep over?"
- **"Has Motics looked at SNOMED CT or any regulatory ontology for Audit Agent? I've been thinking about how to make Audit auditable in a DCB0129-defensible way."** (This is the pitch opener if I haven't gotten to it earlier.)
- "How do you balance clinical safety rigor against the move-fast startup reality?"

**To Salinna**:
- "How do technical decisions get made today? Who owns what?"
- "If I joined in May part-time, what would the first two weeks look like?"
- "What's the code review / shipping culture like?"
- "Biggest piece of tech debt you'd hand to a new senior hire on day one?"

**To the AI Person**:
- "Which agent do you own right now? Walk me through a week in your life on it."
- "How do you currently evaluate your agent's outputs? Human eval, LLM-as-judge, both?"
- "I used LLM-as-judge in clindiff for L4-L7. What do you think about G-Eval or pairwise comparison as alternatives? Curious what you've tried."
- **"If Audit ends up being my territory, where would our products need to interface? What should I expect from Scribe's output, and what should Audit feed back?"**
- **"Have you looked at SNOMED CT for any of your work? Curious what your view is on symbolic approaches vs LLM-only in this domain."**
- "Where do product owners share code and infra here vs keep things isolated?"
- "What's something in your current agent you wish you had time to do properly but can't?"

**To Sales**:
- "What's the most common objection from clinic directors, and what's our best answer?"
- "When a demo goes wrong, what's usually the failure mode — the product, the pitch, or the fit?"
- "How involved do engineers get in customer calls here?"
- "What's one feature you've been asking engineering for that hasn't shipped yet?"

**To Marketing**:
- "What's working best for lead gen right now — content, events, outbound?"
- "Do you want engineers producing technical content, or is that strictly your turf?"
- "How does Motics position against Tortus and Heidi in actual conversations?"

**To anyone (financials / runway)**:
- "Could you share where you are on the Series A timeline? I'd like to understand the runway context."
- "Morgan Stanley MSISV Demo Day was February — has that opened up new investor conversations?"
- "Current paying clinic count and month-over-month growth — is that something you can share?"

---

## 5. Specific Framings to Practice

### 5.1 The "Why Motics" Answer
Anchor on three things, in this order:
1. **Direct methodological fit with my MSc research**: My dissertation is "Explainable Matching via Knowledge Graphs for UK Children's Social Care." Swap the domain and it's exactly what Motics needs for Audit Agent — SNOMED-CT based, explainable, aligned with UK Algorithmic Transparency requirements. The research time isn't a cost to the company; it directly produces IP Motics can use.
2. **Stage fit**: Hire #6 in a product-ownership model means real ownership, not tickets. I want to be a mini-founder for an agent, not an engineer assigned work.
3. **Mission fit**: Clinician burnout is real. I watched SSG automate logistics and saw what happens when a 2-hour manual process drops to 10 minutes. Healthcare admin is the same shape of problem with much higher human stakes.

### 5.2 The "First 90 Days" Pitch (version-dependent on role assignment)

**If they offer Audit Agent** (preferred outcome):
> "If I own Audit, the plan is SNOMED-CT based from day one. Month 1 part-time: survey what's there, select one regulation (GDPR Article 9, or a NICE guideline) as a vertical slice, prototype the extraction → mapping → DL-query pipeline on real Motics notes. Month 2: extend to 2-3 more regulations, start measuring compliance-detection accuracy against manual review. Month 3 leading into full-time in September: integrate with Scribe's output stream, produce a DCB0129-style safety case document as an artifact, and start the conversation with Harvinder on DTAC submission timing. By end of year one, Audit should be both a differentiated product and a regulatory-ready deliverable."

**If they offer co-ownership of Scribe with the AI person**:
> "I'd want to sit with [AI person] week one and map where clindiff's approach complements what they've built. My guess is I'm stronger on evaluation infrastructure, data pipelines, and regulatory-facing artifacts; they're stronger on core modeling and prompting. If that split holds, I take the evaluation harness and reg-facing work, they keep the modeling, and we ship faster together. My Audit KG vision would be a longer-term direction I'd contribute toward once Scribe QA is solid."

**If they offer Phone Agent or platform layer**:
> "Phone Agent actually fits my background even more directly — my Pitwall project was a LangGraph multi-agent system with 20+ agents debating and making decisions, and Viola is a CLI-based orchestration platform with Docker Proxy sandboxing. That's closer to what Phone Agent needs than clindiff is. First 90 days I'd map the current call flow, identify the biggest failure modes, and either extend the orchestration layer or rebuild it depending on what's there. The Audit KG vision stays as a longer-term contribution."

**If they haven't decided yet**:
> "I'm flexible, but I have a ranked preference. Audit fits my MSc research directly and I've got a concrete SNOMED-based approach I'd love to discuss. Scribe co-ownership is strong technical fit given clindiff. Phone Agent fits my multi-agent background. All three work — I just want to understand where the team needs me most before I commit to a plan."

### 5.3 The "Why I'm Not a Risk" Answer
If anyone hints at concerns about a Korean engineer adapting to a London startup:
- 5+ years of cross-functional stakeholder work at SSG (not a siloed engineer)
- Already in London, on Graduate Visa — no sponsorship risk
- UCL MSc is immersion in UK academic and technical culture
- I've shipped production systems with ambiguous requirements before — the Forward Deployed role at SSG is exactly that shape

### 5.4 The Compensation Conversation (If It Comes Up)
Don't volunteer numbers. If pressed:
- Part-time day rate: £400-500/day (equivalent to £45-65/hour)
- Full-time base: £60-75k
- Equity expectation for 6th hire / AI platform ownership: 0.5-1.5%, 4-year vesting, 1-year cliff, EMI scheme
- Want 9-month conversion terms documented in the offer letter

Frame it as: "I'm optimizing for being in the right seat at the right company, not max cash. I'd want the offer letter to lock in the full-time conversion terms so we don't have to renegotiate in September."

---

## 6. Red Flags to Watch For (Mutual Due Diligence)

Things that should shift my willingness to accept:

- **Founder dynamics**: If Harvinder talks over Salinna or corrects her mid-sentence. If their stated priorities contradict.
- **Unclear product ownership**: If they can't tell me which agent I'd own, or if ownership boundaries between people are fuzzy and contested, the autonomy pitch is a lie.
- **AI person hostility**: If they're visibly dismissive of my approach, day-to-day collaboration on interfacing products will be hell.
- **Sales frustration with product delivery**: If Sales drops hints about agents being slow to ship or unstable, that's the current reality I'd inherit for my product too.
- **Role ambiguity**: If I can't get a clear answer on what I'd actually own in the first 3 months, they haven't thought it through and I'd be defining the role under pressure.
- **"We're a family" language**: Often precedes overwork culture or lack of real boundaries. Especially risky in a product-ownership model where nobody can say "that's not my job."
- **Vague answers on Series A**: If nobody can say when or with whom the next round conversation is happening, runway is shorter than they'll admit.
- **Hand-waving on regulatory pathway**: Healthcare without DTAC/MHRA intent isn't serious.
- **No shared infra/practices**: Product ownership works only if there's shared plumbing (auth, logging, deploy, eval infra). If every agent owner has rebuilt everything themselves, complexity will explode.
- **Hiring urgency mismatch**: If they're desperate to get me in by May but vague on what I'd actually own, that's bandwidth panic, not strategy.

Positive signals to confirm:

- Clear answer on which agent I'd own and why (Audit is the obvious candidate given clindiff)
- Named product owner for each existing agent with clean boundaries
- AI person excited about having someone own Audit so their Scribe doesn't have to do double duty as its own QA
- Sales pointing to specific customer requests Audit Agent would unblock
- Concrete numbers on paying clinics, ARR, growth rate (even if small)
- Named investor conversations for Series A
- Named regulatory milestones with dates
- Evidence of shared engineering practices (how deploys work, how incidents are handled, who reviews what)

---

## 7. Roleplay Session Kickoff Prompts

Paste one of these into the tool when starting a session:

**For Harvinder session**:
> "Read the attached Motics 3rd round context. Play Harvinder Power, Co-founder and CEO of Motics, in a 15-20 minute 1:1 with me. You're a medical doctor by training, you care about patient safety and the regulatory pathway, and you're evaluating whether I'll align with the mission and represent Motics well to clinicians. Ask probing questions about my motivation, clinical safety thinking, and long-term fit. Don't go easy — this is a real hiring decision for you. Start with your opening."

**For Salinna + AI Person session**:
> "Read the attached Motics 3rd round context. Play two people: Salinna Abdullah (Co-founder, technical lead, already interviewed me in round 2 and supports my candidacy) and the AI Person (Imperial College London AI PhD, owns one of Motics' AI agents end-to-end as a product, research-rigorous but also ships daily). This is a 25-30 minute technical deep-dive. Salinna is warmer and collaborative; the AI person is academically rigorous and will push me hard on clindiff's methodological choices — especially around empirical validation, evaluation rigor, and whether my proposed improvements (Clinical Ontology, Multi-Judge Ensemble, prose-to-JSON compilation) are serious or hand-wavy. They may cite specific papers or techniques and expect me to engage. Important: at Motics, each person owns one agent as a mini-founder. If I'm hired, I'd likely own Audit Agent. The AI person probably owns Scribe. Explore how our products would interface. Start when ready."

**For Sales + Marketing session**:
> "Read the attached Motics 3rd round context. Play two people: the Sales lead (talks to clinic directors daily, cares about ship velocity and demo quality) and the Marketing lead (cares about brand, content, narrative positioning). This is a 15-20 minute commercial-alignment session. You're not technical but you're sharp — you want to know if I understand the customer, can join a call without confusing them, and can ship visible things you can talk about. Ask business-savvy questions, including some culture-fit probes. Informal tone, but you're still evaluating. Start when ready."

**For full team group session** (if the 3rd round is one combined session):
> "Read the attached Motics 3rd round context. Play all 5 current Motics team members (Harvinder, Salinna, AI Researcher, Sales, Marketing) in a combined 60-minute 3rd round group interview. Different team members chime in on different questions based on their role. Harvinder opens, Salinna and the AI researcher ask technical questions, Sales and Marketing ask commercial and culture questions. Make it feel like a real group dynamic — people building on each other, occasionally disagreeing, different energies. Start when ready."

**For feedback mode** (after any session):
> "Now switch to feedback mode. Evaluate my performance in the session above across: (1) clarity and specificity of my answers, (2) technical depth, (3) cultural fit signal with each individual team member, (4) questions I asked back, (5) any red flags I gave off. Be direct and specific — I want to improve, not be reassured."

---

**This document is for simulation only. The AI playing interviewers does not need to pretend everything is accurate about the real Motics team — it's a practice scaffold.**
