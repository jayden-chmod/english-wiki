# Session 001 Summary
**Source:** `original/session_001.md`
**Date:** 2026-04-04
**Time:** 1:40 PM – 6:32 PM (BST)
**Topic:** First day onboarding + Audit Agent architecture discussion

---

## Conversation Overview
A first-day conversation between a new full-stack engineer (User) and Salinna (CTO). Started with casual small talk about the development setup, then evolved into a deep technical discussion about redesigning the Audit Agent with SNOMED CT ontology validation and a shared clinical data microservice.

---

## Key Topics Discussed

### 1. Development Environment & Docker
`[U-001]` `[U-002]`
- User is getting familiar with the setup; already comfortable with Docker from personal projects.

### 2. Audit Agent — Ontology Validation Idea
`[U-003]` `[U-005]` `[U-006]`
- User proposed integrating **SNOMED CT** ontology into the Audit Agent to:
  - Ground medical terminology (validate/correct disease names against ontology)
  - Ensure output consistency in structured clinical records
  - Track overdue actions and notify stakeholders
  - Periodically generate improvement reports for audit readiness
- Salinna recommended **SNOMED CT** over FHIR for UK clinical settings; FHIR is more about data exchange.

### 3. Agent Grouping by Domain
`[U-007]`
- User proposed grouping agents by function:
  - **Clinical layer**: Scribe Agent + Audit Agent (medical records & documentation)
  - **Admin layer**: Phone Agent + Email Agent (scheduling, patient comms)
  - **Unclear**: Billing Agent — touches both clinical data (diagnosis codes) and financial output
- Salinna suggested Billing may act as a **bridge layer** between the two.

### 4. Shared Clinical Data Layer (Microservices Architecture)
`[U-008]` `[U-009]` `[U-010]` `[U-011]`
- User proposed an **independent clinical data service** that all agents access via endpoints (not direct DB access).
- Rationale: prevents uncontrolled data modification, enforces data integrity.
- Phone Agent exception `[U-010]`: doesn't need full clinical data; handles reservations/cancellations only.
- For Phone Agent, Salinna suggested **read-only access to a limited subset** (e.g., booking data, contact info).
- User proposed a **read-only service account**; Salinna refined this to a **GCP IAM role** with restricted permissions enforced at infrastructure level.

### 5. GCP IAM
`[U-012]`
- User has personal experience only but confident they can pick it up from docs.
- Salinna offered to share internal examples.

### 6. Casual Close — Team Lunch
`[U-013]` `[U-014]` `[U-015]`
- Team does Friday lunches; user joined the group heading to a Thai restaurant.

---

## Grammar Patterns to Review (Recurring Issues)

| Issue | Example | Fix | Source |
|-------|---------|-----|--------|
| Missing article "a/the" | "development environment" | "the development environment" | `[U-001]` `[U-011]` |
| 3rd person singular | "agent need", "every agent access" | "needs", "accesses" | `[U-010]` `[U-009]` |
| Wrong preposition | "search on ontology", "introduce on" | "search the ontology", "introduce into" | `[U-006]` `[U-003]` |
| Passive voice missing | "hasn't done yet" | "hasn't been done yet" | `[U-006]` |
| Spelling typos | "prefered", "independatnt", "billimg" | "preferred", "independent", "billing" | `[U-002]` `[U-008]` `[U-007]` |
| Pronoun inconsistency | "They're" for singular agent | "It handles" | `[U-010]` |
| Wrong word choice | "acessibility", "develop it", "admin works" | "access", "pick it up", "admin tasks" | `[U-011]` `[U-012]` `[U-007]` |
| Korean habits | trailing "~", "?" instead of "!" | drop "~", use "!" | `[U-015]` `[U-014]` |
| Redundant article | "an another" | "another" | `[U-011]` |
| Sentence fragment | "Just personal use." | "Just for personal projects." | `[U-012]` |
