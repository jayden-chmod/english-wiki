# S5 — Pitwall / CogEn (Project Walkthroughs + Production Challenge)

Unlike S1–S4 (behavioral STAR), S5 questions are mostly technical project explanations. Three of the four are walkthroughs (Q13, Q14, Q16); only Q22 fits a STAR-style answer naturally.

Mapped questions (in `interview_qa_all.md`):
- Q13 — Walk me through CogEn (Project walkthrough)
- Q14 — Belief change modeling deep-dive (Technical deep-dive)
- Q16 — Walk me through Pitwall / AI safety in agent systems (Project + safety)
- Q22 — Hardest part of applying AI to production (STAR-style)

Project context: CogEn (personal project, neuro-symbolic conversational system) and Pitwall (multi-agent finance debate platform).

Structure used for walkthroughs: **What it does → How it works → Why it matters for Motics.**

---

## Version A — Q13 (CogEn Walkthrough)

**Structure:** What → Why exists → How it works → Why it matters for Motics

**Script**

"CogEn is a conversational system I'm building around belief change modeling. The core idea is simple. Most AI systems treat knowledge as static. I wanted to build something that reflects reality. Beliefs change over time."

"It works in three steps. When you say something, the system transforms your input into a structured format. It then retrieves relevant context from a knowledge graph using both vector embedding search and node traversal. The system generates a response from that. After responding, the knowledge graph updates asynchronously. Existing beliefs get reinforced, weakened, or terminated based on what you said."

"Why this matters for Motics. Your Scribe Agent already extracts symptoms and duration from conversations. If those are structured into a knowledge graph, ICD suggestion becomes a graph search problem. Combined with patient history and medical knowledge, the system can make more specific decisions. The graph also enforces hard clinical rules. What must be present. What cannot. Those are cases a probabilistic model cannot reliably catch."

"I read that your Scribe Agent has reached 90% accuracy in ICD coding. The remaining 10% is where a knowledge graph can help."

**Key signals:** Connects directly to Motics product. Closing line ("the remaining 10%") is the hook.

---

## Version B — Q14 (Belief Change Modeling Deep-Dive)

**Structure:** The mechanic → The principle (LLM as language, not reasoning) → Tactful close

**Script**

"When a message comes in, CogEn structures the input the same way data is stored in the graph. Then it runs both embedding-based search and node traversal to pull the relevant belief states. I'm also adding graph structure embedding to make retrieval more precise."

"Based on what's retrieved, the graph updates. New knowledge gets created. Existing beliefs get reinforced, weakened, or terminated. The LLM makes those judgments."

"But I've worked to minimise how much depends on LLM judgment. Just passing information to the LLM and letting it decide is no different from RAG. LLM assistance at intermediate steps is fine. But the final reasoning should be explainable, traceable, and replicable. The last LLM call should only handle language."

"I'm still deciding whether to build a service around this or release it as open source. So I hope you'll understand if I stop the detail there."

**Key signals:** "The last LLM call should only handle language" is the philosophical anchor. Open-source-vs-service line is a clean cap that protects unfinished thinking without sounding evasive.

---

## Version C — Q16 (Pitwall Walkthrough + AI Safety)

**Structure:** What Pitwall is → Safety mechanism → Why it's insufficient for healthcare → What healthcare needs

**Script**

"Pitwall is a platform where twenty agents debate and vote on financial topics. Each agent has its own investment philosophy and accesses news, analyst target prices, and market data through dedicated MCP servers. Based on that data, each agent produces an argument, a target price, and a vote. At the topic deadline, the system evaluates all results."

"On safety. The agent pipeline runs on LangGraph. When an agent produces output, the system validates it against a defined schema. If it fails, it retries. After a few retries, it stops and sends me a Telegram alert."

"Schema-level validation is important. But I think healthcare needs more. Schema validation checks structure. It doesn't check whether the content is clinically valid. It means an output can pass schema validation and still be wrong."

"That's where ontologies and knowledge graphs come in. They validate whether the relationships between clinical facts are consistent. So safe AI in healthcare needs both. Schema validation and ontology validation, both passing, before the output reaches a clinician."

**Key signals:** Pitwall is the proof-of-concept; the punchline is what healthcare needs *beyond* it. Sets up the Motics framing.

---

## Version D — Q22 (Production Difficulty / STAR-style)

**Structure:** STAR with three-pillar Action

**Script**

**[Headline]**
"The hardest part of applying AI to production is consistency. LLM systems produce variable output. Controlling that took significant effort. I'll walk through the three pieces that mattered most."

**[S+T]**
"When I built Pitwall, I had twenty agents producing structured arguments, target prices, and votes. Any agent giving a malformed output broke the downstream evaluation. So consistency wasn't a nice-to-have. It was the central engineering risk."

**[Action]**

"**Prompt design.** Prompts need calibration. Too specific and the model gets trapped in the instruction. Too general and you lose control. Finding the right balance requires iteration. There's no shortcut. But if your test suite is concrete enough, an agent loop can handle much of that iteration for you."

"**Context management.** When you give a model too much at once, performance drops and hallucination increases. The solution is decomposition. Decisions that can be made independently should be. You inject only what is relevant at each step. By the time the final decision is made, the model is working with refined, focused information."

"**Structured output.** Rather than using the model's raw output directly, I define a template in advance. The model fills in pieces, validated against a schema, and the server assembles the final response by injecting those pieces into the template. The model never owns the shape of the output. The system does."

**[Result]**
"Pitwall is built exactly this way. The pipeline runs in four to five steps. Context not needed at a given step is stored separately and injected only when required. Every output is schema-checked before the next step runs. That's what made it reliable enough for production."

**Key signals:** Three explicit pillars (prompt / context / structured output). Pitwall as concrete proof.

---

## Quick comparison

| Version | Question fit | Format | Closing focus |
|---------|-------------|--------|--------------|
| A — CogEn | Q13 | Walkthrough | Motics Scribe Agent application |
| B — Belief change | Q14 | Deep-dive | "LLM should only handle language" |
| C — Pitwall + safety | Q16 | Walkthrough + safety | Healthcare needs schema + ontology |
| D — Production | Q22 | STAR | "Reliable enough for production" |

---

## Practice notes

- A and C both close with Motics-specific framing. That's deliberate. They turn a technical answer into "and here's why I'm useful to you."
- B's open-source-vs-service close is a clean exit. It's not evasive — it signals you've thought about commercial implications, while not over-promising.
- D is the only behavioral-style answer in S5. The three pillars (prompt / context / structured output) work as a memorable spoken structure.
- These four are likely to come up in *technical* interview blocks rather than behavioral ones. Pace yourself accordingly. Don't rush like a behavioral STAR; let technical answers breathe.
- Q13 and Q16 may be follow-ups to "what would you improve about our product?" (Q6) — the Scribe Agent connection is consistent across all three.
