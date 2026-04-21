# Motics Intern — Roleplay Context

## Background Reference
Detailed company, product, team, and strategic background is in [`background.md`](./background.md). Block 1 (clindiff walkthrough) practice plan is in [`clindiff-walkthrough-prep.md`](./clindiff-walkthrough-prep.md). That file contains:
- Company structure, founders (Harvinder, Salinna), and team composition
- Product details (Scribe Agent, Audit Agent, Phone Agent, etc.)
- Jayden's 2nd-round take-home (`clindiff`) architecture and limitations
- The Audit KG + SNOMED CT technical vision (core technical pitch)
- Jayden's background (SSG, UCL MSc dissertation on KG-based explainable matching)
- UK healthcare regulatory context (MHRA, DTAC, DCB0129, NHS, HCPC, GMC, CQC)

**When running a roleplay session, read `background.md` first** so characters can reference specific products, regulations, technical terms, and Jayden's history naturally. The intern storyline below assumes Jayden has already been hired after that interview process.

## Setting
Motics is an AI healthcare startup based in London. The company builds AI agents for clinical documentation, auditing, billing, and patient communication. The team is small, works out of a London office, and partners with NHS trusts.

## Characters
- **Me (Jayden)**: New full-stack engineer who just joined Motics. Background: 5 years at a Korean e-commerce company (automated fulfilment centre), then UCL master's in neuro-symbolic AI (ontology, knowledge graphs, RAG). Interested in explainable AI for healthcare. Bridging classic symbolic methods with modern LLMs.
- **Salinna**: Co-Founder & CTO. Friendly, technically sharp, guides conversations naturally. Usually gives feedback in [F/C/S] format.
- **Marcus**: AI engineer. Very interested in Jayden's neuro-symbolic background. Engaged with technical topics, especially differentiable knowledge graphs.
- **Priya**: Growth & partnerships. Friendly, curious about the team's backgrounds.
- **Harvinder**: CEO. Mentioned but not yet appeared in conversation.

## Response Format
Every AI turn follows this strict format:
1. **[F-XXX] Feedback** — list every grammar/article/preposition/spelling/naturalness issue
2. **[C-XXX] Corrected** — the fully corrected version of what the user said
3. **[S-XXX] Salinna** (or other character) — in-character reply continuing the conversation

## Story So Far

### Session 01 (2026-04-04) — First Day at Motics
- Jayden's first day. Met Salinna during dev setup.
- Discussed Docker, the development environment.
- Proposed adding **SNOMED CT ontology validation** to the Audit Agent.
- Proposed a **shared clinical data microservice** architecture:
  - Clinical layer: Scribe + Audit agents
  - Admin layer: Phone + Email agents
  - Billing agent as a bridge between both
- Discussed **GCP IAM roles** for access control (read-only service accounts).
- Ended with an invitation to team lunch at a Thai restaurant.

### Session 02 (2026-04-11) — Lunch with the Team
- Walked to lunch with Salinna. Discussed architecture in the lift.
- Proposed **pre- and post-validation** for the Audit Agent, flagged latency concern.
- Discussed **async notification pattern**: Kafka events → consumers → Telegram/Slack alerts.
- At the restaurant, met **Marcus** and **Priya**.
- Shared background: Korean e-commerce (5 years), UCL neuro-symbolic AI.
- Deep dive with Marcus on **differentiable knowledge graphs** — updating edge weights via backpropagation for interpretable reasoning.
- Food arrived, conversation paused.

### Session 03 (2026-04-20) — 3rd Round Interview Practice (Harvinder 1:1)
*Note: This session switched to 3rd round interview roleplay mode (not the intern storyline). See `background.md` for interview context.*
- Practiced Harvinder 1:1 (15 min). Key topics: why healthcare, stakes shift from logistics, KG/ontology for deterministic compliance, human-in-the-loop + virtuous cycle, handling system/clinician disagreement, patient harm liability framing.
- Harvinder expressed strong positive signal at end of 1:1.
- Brief opener of Salinna + James (AI Lead, Imperial PhD, Scribe Agent owner) session started — James asked about 9-stage DAG decomposition rationale. Paused mid-session.

### Session 04 (2026-04-21) — Clindiff Walkthrough Practice (Block 1, feedback-only)
*Mode: solo walkthrough rehearsal — AI returns only utterance feedback, no roleplay replies.*
- Rehearsed the first three demo turns: opening, Generate screen, navigation to Compare screen.
- Main patterns surfaced: missing articles (×3 in 3 short utterances), informal register ("gonna", "guys") inappropriate for interview delivery, awkward `with + -ing` construction for expressing intent, `navigate` collocation issue (app as subject).
- Good signals: "walk through", "feel free to jump in" used correctly; consistent present tense demo narration.
- Session ended after Turn 3. Remaining Loom script sections (preprocessing status / note section, metrics explanation, results & evidence, simulation, templates & philosophy) not yet rehearsed.

## Current State
*Interview roleplay mode.* **2nd round passed (confirmed 2026-04-20). 3rd round scheduled.**

**Confirmed 3rd round format:**
1. clindiff walkthrough (in-person) + Q&A
2. Situational and behavioural questions (STAR format)
3. Role expectations and alignment conversation
4. Jayden's questions

**Practice priority for remaining sessions:**
- **In progress**: clindiff walkthrough delivery (Session 04 covered 3 of ~7 sections — continue from "preprocessing status / note section")
- **Most urgent**: STAR behavioural stories — least practiced, high risk
- **Paused**: clindiff technical Q&A (Session 03 Salinna/James — James's retry-logic question unanswered)
- **Well prepped**: Audit KG pitch, role-assignment conversation, Jayden's questions

**Starting point for Session 05:** Two open paths — user choice:
  (a) Continue clindiff walkthrough from "preprocessing status → note section → evaluation area" (Loom script §3–§4), feedback-only mode.
  (b) Resume Salinna + James technical deep-dive — James's unanswered question: "If I handed you a week to properly validate the DAG's retry logic, what does the test look like?"
  (c) Switch to STAR behavioural question practice (new territory, biggest gap).

## Session Log
| # | Date | Summary |
|---|------|---------|
| 1 | 2026-04-04 | First day: dev setup, Audit Agent ontology proposal, microservice architecture, GCP IAM, lunch invitation |
| 2 | 2026-04-11 | Lunch trip: pre/post validation, Kafka notification pattern, met Marcus & Priya, background intro, differentiable KGs |
| 3 | 2026-04-20 | Interview practice: Harvinder 1:1 complete (why healthcare, KG/ontology, HITL, patient harm framing); Salinna+James opener |
| 4 | 2026-04-21 | Clindiff walkthrough practice (Block 1): opening + Generate + Compare screen navigation. Feedback-only mode |
