# Session 002 Summary
**Source:** `original/session_002.md`
**Date:** 2026-04-11
**Topic:** Going to lunch + architecture discussion (pre/post validation, Kafka, notification layer) + team intro (Marcus, Priya) + background/skills intro

---

## Conversation Overview
Continuation of the first day. Salinna invited the user to team lunch at a Thai restaurant. On the way (lift), they discussed audit agent architecture — specifically pre/post validation latency, Kafka-based async notifications, and event-driven decoupling. At the restaurant, the user was introduced to Marcus (AI engineer) and Priya (growth/partnerships) and shared their background: 5 years at a Korean e-commerce company optimising fulfilment, then a UCL master's in neuro-symbolic AI (ontology, knowledge graphs, RAG). Conversation ended with Marcus probing about differentiable knowledge graphs.

---

## Key Topics Discussed

### 1. Accepting Lunch Invitation
`[U-001]` `[U-002]`
- User accepted Salinna's lunch invitation; noted they hadn't met the team yet.

### 2. Audit Agent — Pre/Post Validation
`[U-003]` `[U-004]` `[U-005]`
- User asked why the current architecture was chosen (modular/NHS compliance reasons).
- User proposed adding **pre- and post-validation** to the audit agent.
- Concern: pre-validation may **introduce latency** in the critical path.
- Proposed solution: measure latency first, then build a **notification system** to alert stakeholders asynchronously if not tolerable.

### 3. Async Notification — Kafka & Telegram
`[U-006]` `[U-007]` `[U-008]` `[U-009]`
- User has experience sending **Telegram messages asynchronously** via Kafka events.
- Design: **publish failure events** after task completion; a separate consumer handles routing to Telegram/Slack/etc.
- Distinction: event-driven for complex multi-task flows; direct trigger for simple setups.
- Agreed on **decoupled event-bus pattern** for Motics (multiple failure modes → different stakeholders per event type).

### 4. Team Introduction
`[U-010]`
- User introduced themselves: full-stack engineer, UCL master's, 5 years Korean experience.
- Marcus reacted positively to neuro-symbolic AI background — highly relevant to Motics' LLM-heavy stack.

### 5. Background — Korean Experience & UCL
`[U-011]` `[U-012]` `[U-013]`
- Worked at **e-commerce company** in Korea for ~5 years, focused on **optimising automated fulfilment centres**.
- Moved to London to study **explainable AI** at UCL; pivoted to healthcare as the field where XAI is most needed.
- Master's covered: neuro-symbolic AI, ontology, knowledge graphs, RAG systems.

### 6. Research Interest — Differentiable Knowledge Graphs
`[U-014]` `[U-015]` `[U-016]` `[U-017]` `[U-018]`
- User positioned themselves as a bridge between classic symbolic AI (UCL) and modern ML/LLMs.
- Research interest: **differentiable (weighted) knowledge graphs** — updating edge weights via backpropagation to keep reasoning interpretable.
- Cited some existing literature on the topic but hadn't reviewed it thoroughly yet.

---

## Grammar Patterns to Review (Recurring Issues)

| Issue | Example | Fix | Source |
|-------|---------|-----|--------|
| Plural "knowledge graphs" | "knowledge graph" | "knowledge graphs" | `[U-012]` `[U-015]` `[U-016]` `[U-017]` `[U-018]` |
| Missing article (a/the) | "at e-commerce company", "a event" | "at an e-commerce company", "an event" | `[U-006]` `[U-009]` `[U-013]` |
| Embedded question word order | "why do you choose" | "why you chose" | `[U-003]` |
| "dig on" → preposition | "dig on" | "dig into" | `[U-017]` |
| "something other" (Korean habit) | "something other" | "something else" | `[U-007]` |
| Past participle missing | "haven't check" | "haven't checked" | `[U-018]` |
| Wrong verb form | "I designed it publishing" | "I designed it to publish" | `[U-008]` |
| "emphasises on" | "emphasises on X" | "emphasises X" (no preposition) | `[U-015]` |
| Missing "for" before time duration | "worked about 5 years" | "worked for about 5 years" | `[U-012]` |
| "differential" vs "differentiable" | "differential knowledge graph" | "differentiable knowledge graphs" | `[U-018]` |
| Spelling/typos | "queation", "simlle", "knlw", "asyncronisly" | "question", "simple", "know", "asynchronously" | Multiple |
