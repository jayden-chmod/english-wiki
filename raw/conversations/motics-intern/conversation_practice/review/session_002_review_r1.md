# Session 002 — 1차 리뷰 결과
**리뷰 날짜:** 2026-04-11
**원본:** `original/session_002.md`

---

## 결과 요약

| 구분 | 문항 수 |
|------|--------|
| 정답 ✓ | 1 |
| 부분 정답 △ | 5 |
| 오답 ✗ | 12 |

---

## 문항별 결과

| 문항 | 결과 | 핵심 실수 포인트 |
|------|------|----------------|
| U-001 | △ | "sounds good" → "sounds great!" (어색한 온도, 마침표 → 느낌표) |
| U-002 | ✓ | — |
| U-003 | ✗ | "why do you choose" → "why you chose" (내포 의문문 어순 + 과거시제) |
| U-004 | △ | "pre and post validation" → "pre- and post-validation" (하이픈); "cause" → "introduce" |
| U-005 | ✗ | "nees" 오타, "how it takes" → "how long it takes", "kz" → "it's", "make notifying system" → "build a notification system" |
| U-006 | ✗ | "experience of" → "experience", "telegram" → "Telegram" (대문자), "asyncronisly" 오타, "Doe" → "Does", 관사 "a" 누락 |
| U-007 | ✗ | "queation" 오타, 이중 "was" 구조 오류, "something other" → "something else", "wasn't it" → "weren't you" |
| U-008 | ✗ | "I designed it publishing" → "to publish", "after it ended" → "after it finished", 관사 누락, "fail event" → "failure events", "messaged" → "send a notification", "simlle" 오타, "telegram event" → "a Telegram message directly" |
| U-009 | ✗ | "is and will be involved" → "does and will involve", "a event" → "an event", "decouple involved tasks" → "decouple the tasks" |
| U-010 | △ | "joined" → "I've joined" (현재완료), "Fullstack Developer" → "a full-stack developer" (관사 + 소문자 + 하이픈) |
| U-011 | △ | double "and" 구조 (run-on), "UCL masters" → "my master's at UCL" |
| U-012 | ✗ | "neuro symblic" → "neuro-symbolic" (오타 + 하이픈), "knowledge graph" → "knowledge graphs" (복수), "worked about 5 years" → "worked for about 5 years" |
| U-013 | ✗ | "ontology grounding helpful" → "ontology grounding is helpful" (동사 누락), 관사 "an" 두 곳 누락 |
| U-014 | △ | "health care" → "healthcare", "requires them" → "needs it" (단수 대명사) |
| U-015 | ✗ | "emphasises on" → "emphasises" (전치사 불필요), "study" → "studied" (과거), "knowledge graph" → "knowledge graphs", "Rag" → "RAG" |
| U-016 | ✗ | "expert" → "experts", "knowledge graph" → "knowledge graphs", "knlw" 오타, "classic method from" → "the classic methods at", "will integrate" → "want to integrate" |
| U-017 | ✗ | "dig on" → "dig into", "weight" → "weights", "by a backpropagation" → "via backpropagation", 관사 "the" 누락 |
| U-018 | ✗ | "differential" → "differentiable", "knowledge graph" → "knowledge graphs", "haven't check" → "haven't checked" |

---

## 재시험 대상 문항 (오답 + 부분 정답)

### 오답 ✗ (12문항)

- **U-003** — "Yeah, I saw it. Can I ask why do you choose that architecture?"
- **U-005** — "Hmm, I think we nees to check how it takes, whether kz tolerable or not..."
- **U-006** — "Yeah, I have experience of sending telegram message asyncronisly. Doe your app have notification layer?"
- **U-007** — "I thought your queation was checking async tasks was polling or something other, wasn't it?"
- **U-008** — "If it had many things to do, I designed it publishing an event after it ended..."
- **U-009** — "I agree. It is and will be involved many tasks. So we just publish a event and decouple involved tasks."
- **U-012** — "I studied neuro symblic AI like ontology and knowledge graph... I worked about 5 years before I came here!"
- **U-013** — "Yeah, I also think ontology grounding helpful. I worked at e-commerce company..."
- **U-015** — "Symbolic AI emphasises on strict rules and we also study about knowledge graph and Rag system."
- **U-016** — "Because they are expert in symbolic AI and knowledge graph but don't knlw much about the latest tech..."
- **U-017** — "I want to dig on weighted knowledge graph updating weight by a backpropagation..."
- **U-018** — "There are some articles about differential knowledge graph but I haven't check thoroughly."

### 부분 정답 △ (5문항)

- **U-001** — "That sounds good." → 수락 표현 온도 + 구두점
- **U-004** — "pre and post validation" 하이픈 + "introduce latency"
- **U-010** — 현재완료 + 관사 + "full-stack"
- **U-011** — run-on (double "and") + "my master's at UCL"
- **U-014** — "healthcare" (한 단어) + "it" (단수 대명사)

---

## 반복 오류 패턴

| 패턴 | 해당 문항 |
|------|---------|
| 복수형 누락 ("knowledge graphs") | U-012, U-015, U-016, U-017, U-018 |
| 관사 누락 (a/an/the) | U-006, U-009, U-013, U-017 |
| 오타 | U-005, U-006, U-007, U-008, U-012, U-015, U-016 |
| 잘못된 전치사 ("dig on", "emphasises on") | U-015, U-017 |
| 한국어식 표현 ("something other") | U-007 |
| 내포 의문문 어순 | U-003 |
| 현재완료 vs 단순과거 | U-010, U-015 |
| 기술 용어 정확도 ("differential" vs "differentiable") | U-018 |
