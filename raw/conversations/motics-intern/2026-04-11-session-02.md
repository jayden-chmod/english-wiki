# Session 002 — Lunch with the Team
**Date:** 2026-04-11
**Topic:** Going to lunch, architecture discussion (pre/post validation, Kafka, notification layer), meeting Marcus & Priya, background intro

---

[S-001] **Salinna:** *walks over* Hey — we're about to head out for lunch. A few of us are going to the Thai place down the road. Want to come? Good chance to meet the rest of the team.

[U-001] **User:** That sounds good. I haven't had a chance to meet everyone yet.

[F-001] **Feedback:**
- "That sounds good." → natural, but "That sounds great!" is warmer and more common for accepting an invitation.

[C-001] **Corrected:** "That sounds great! I haven't had a chance to meet everyone yet."

---

[S-002] **Salinna:** Ha, yeah — it's a small team but everyone's pretty heads-down in the mornings. Lunch is honestly the best time to actually talk to people. We've got folks from the clinical side too, not just engineers. Ready to head out?

[U-002] **User:** Yeah, let's go!

[F-002] **Feedback:**
- No issues. Short, energetic, and perfectly natural.

[C-002] **Corrected:** No correction needed.

---

[S-003] **Salinna:** *grabs jacket* Great — Marcus and Priya are already waiting by the lift. Come on.

*In the lift*

So, first impressions of the codebase so far? Did you get a chance to look at anything this morning?

[U-003] **User:** Yeah, I saw it. Can I ask why do you choose that architecture?

[F-003] **Feedback:**
- "why do you choose" → "why you chose" — two issues:
  1. Embedded questions use statement word order, not question word order
  2. Past tense needed — the architecture was already decided

[C-003] **Corrected:** "Yeah, I saw it. Can I ask why you chose that architecture?"

---

[S-004] **Salinna:** Oh, good question. Honestly, a lot of it came down to the clinical partners we were targeting early on. UK NHS trusts are very particular about data residency and audit trails, so we needed something modular — each agent isolated, with clear logs of what touched what. The monolith approach just felt too risky for a regulated environment. What part caught your eye?

[U-004] **User:** I mainly checked the audit agent. I think we need pre and post validation but I'm concerned that pre-validation may cause service latency.

[F-004] **Feedback:**
- "pre and post validation" → "pre- and post-validation" (hyphens needed for compound modifiers)
- "cause service latency" → "introduce latency" (more natural in engineering contexts)

[C-004] **Corrected:** "I mainly checked the audit agent. I think we need pre- and post-validation, but I'm concerned that pre-validation might introduce latency."

---

[S-005] **Salinna:** That's exactly the right tension to flag. Pre-validation is safer — you catch bad data before it does any damage — but yeah, it adds a synchronous step in the critical path. What's your instinct? Async pre-validation with a fallback, or just accept the latency if it stays under a threshold?

[U-005] **User:** Hmm, I think we nees to check how it takes, whether kz tolerable or not. If it's not, we need to make notifying system to alert stakeholders later.

[F-005] **Feedback:**
- "nees" → "need" (typo)
- "how it takes" → "how long it takes" (missing "long")
- "whether kz tolerable" → "whether it's tolerable" (typo: "kz" → "it's")
- "make notifying system" → "build a notification system" ("make" too vague; "notifying system" → "notification system")

[C-005] **Corrected:** "Hmm, I think we need to check how long it takes, whether it's tolerable or not. If it's not, we need to build a notification system to alert stakeholders later."

---

[S-006] **Salinna:** That's sensible — measure first, then decide. And the async notification idea is actually interesting — if pre-validation fails or is too slow, you flag it and let a human review it rather than blocking the whole pipeline. Have you worked with something like that before — like a dead-letter queue or an alerting layer?

[U-006] **User:** Yeah, I have experience of sending telegram message asyncronisly. Doe your app have notification layer?

[F-006] **Feedback:**
- "experience of sending" → "experience sending" or "experience with sending"
- "telegram message" → "Telegram messages" (brand name + plural)
- "asyncronisly" → "asynchronously" (spelling)
- "Doe your app" → "Does your app" (typo: missing "s")
- "have notification layer" → "have a notification layer" (missing article)

[C-006] **Corrected:** "Yeah, I have experience sending Telegram messages asynchronously. Does your app have a notification layer?"

---

[S-007] **Salinna:** Oh nice — Telegram bots are actually really common in healthtech for internal alerting. We don't have a formal notification layer yet, honestly. Right now it's mostly just Slack webhooks for internal errors. But if you're thinking about building something more structured — routing alerts by severity or agent — that could be a solid contribution. Was your Telegram setup event-driven or more of a polling approach?

[U-007] **User:** I thought your queation was checking async tasks was polling or something other, wasn't it?

[F-007] **Feedback:**
- "queation" → "question" (typo/spelling)
- "your question was checking async tasks was polling" → double "was" creates broken structure; natural: "your question was asking whether the async check used polling or something else"
- "something other" → "something else" (Korean-influenced: "다른 것" → "something else")
- "wasn't it?" → "weren't you?" (subject is "you")

[C-007] **Corrected:** "I thought your question was asking whether the async task check used polling or something else — weren't you?"

---

[S-008] **Salinna:** Oh — you're right, sorry, I was! I jumped ahead. So, was it event-driven or polling-based? Because with Kafka you're typically consuming events, but I wasn't sure if your original setup triggered the Telegram message on an event or if something was periodically checking task status.

[U-008] **User:** If it had many things to do, I designed it publishing an event after it ended, and consumer consumed fail event and messaged. But it was simlle, so I just triggered telegram event.

[F-008] **Feedback:**
- "If it had many things to do" → "If there were many tasks involved" (more natural)
- "I designed it publishing" → "I designed it to publish" (infinitive after "designed it to")
- "after it ended" → "after it finished" (more natural)
- "consumer consumed fail event" → "a consumer would handle the failure events" (articles missing; "fail" → "failure")
- "and messaged" → "and send a notification" ("messaged" too informal/ambiguous)
- "simlle" → "simple" (typo)
- "triggered telegram event" → "triggered a Telegram message directly" (article; "event" → "message")

[C-008] **Corrected:** "If there were many tasks involved, I designed it to publish an event after it finished, and a consumer would handle the failure events and send a notification. But since it was simple, I just triggered a Telegram message directly."

---

[S-009] **Salinna:** Got it — so you kept it pragmatic. Event-driven for complex flows, direct trigger for simple ones. For our use case I think we'd want the event-driven approach — the audit agent has multiple failure modes and we'll probably want different stakeholders notified depending on the type. Does that match how you'd think about it?

[U-009] **User:** I agree. It is and will be involved many tasks. So we just publish a event and decouple involved tasks.

[F-009] **Feedback:**
- "It is and will be involved many tasks" → "It does and will involve many tasks" (verb: "involve"; correct tense)
- "publish a event" → "publish an event" ("an" before vowel sound)
- "decouple involved tasks" → "decouple the tasks" (cleaner)

[C-009] **Corrected:** "I agree. It does and will involve many tasks, so we should just publish an event and decouple the tasks."

---

[S-010] **Salinna:** Exactly — one event bus, multiple consumers, each handling their own responsibility. Clean separation. *pushes open the restaurant door* Right, here we are. After you — grab a seat wherever, I'll introduce you to the others.

*Inside the Thai restaurant. Marcus and Priya are already seated.*

"Everyone, this is Jayden — he just joined us as a full-stack engineer. Jayden, this is Marcus, our AI engineer, and Priya, who handles growth and partnerships. Oh, and Harvinder — our CEO — should be joining us in a minute."

[U-010] **User:** Nice to meet you! I'm Jayden and joined this team as Fullstack Developer. I hope we work well together. Looking forward to working with you all!

[F-010] **Feedback:**
- "joined this team as Fullstack Developer" → "I've joined the team as a full-stack developer" (present perfect; article "a"; "this" → "the"; lowercase hyphenated)

[C-010] **Corrected:** "Nice to meet you! I'm Jayden and I've joined the team as a full-stack developer. I hope we work well together — looking forward to working with you all!"

---

[S-011] **Marcus:** Oh nice, welcome! Full-stack — so you're touching both frontend and the agent backend? We could use the extra hands on the pipeline side, honestly.

[U-011] **User:** I was in Korea and came here and studied at UCL masters.

[F-011] **Feedback:**
- "I was in Korea and came here and studied" → double "and" is run-on; use "then": "I was in Korea, then came here to do my master's"
- "UCL masters" → "my master's at UCL" (possessive apostrophe; article "my")

[C-011] **Corrected:** "I was in Korea, then came here to do my master's at UCL."

---

[S-012] **Priya:** Oh wow, UCL — that's impressive! What did you study?

[U-012] **User:** I studied neuro symblic AI like ontology and knowledge graph. Right, there are many good developers in Korea. I worked about 5 years before I came here!

[F-012] **Feedback:**
- "neuro symblic" → "neuro-symbolic" (hyphen + spelling)
- "knowledge graph" → "knowledge graphs" (plural)
- "I worked about 5 years" → "I worked for about 5 years" (preposition "for" required before time durations)

[C-012] **Corrected:** "I studied neuro-symbolic AI — things like ontology and knowledge graphs. Right, there are many good developers in Korea. I worked for about 5 years before I came here!"

---

[S-013] **Marcus:** Wait — neuro-symbolic AI? That's actually really relevant to what we're building. We've been leaning heavily on pure LLM stuff, but ontology grounding is something we keep talking about and never getting to. What kind of projects were you working on in Korea?

[U-013] **User:** Yeah, I also think ontology grounding helpful. I worked at e-commerce company and my main job was optimising automated fulfilment centre.

[F-013] **Feedback:**
- "think ontology grounding helpful" → "think ontology grounding is helpful" (missing verb "is")
- "at e-commerce company" → "at an e-commerce company" (missing article "an")
- "optimising automated fulfilment centre" → "optimising an automated fulfilment centre" (missing article "an")

[C-013] **Corrected:** "Yeah, I also think ontology grounding is helpful. I worked at an e-commerce company and my main job was optimising an automated fulfilment centre."

---

[S-014] **Marcus:** Oh interesting — so logistics and supply chain? That's pretty different from healthcare. What drew you to the medical AI space then?

[U-014] **User:** I came to London to study explainable AI and I think health care requires them most directly. So I decided to move into healthcare.

[F-014] **Feedback:**
- "health care requires them" → "healthcare needs it the most" ("health care" → "healthcare"; "them" → "it" since explainable AI is singular; "most directly" → "the most")

[C-014] **Corrected:** "I came to London to study explainable AI, and I think healthcare needs it the most. So I decided to move into healthcare."

---

[S-015] **Marcus:** That actually makes a lot of sense — explainable AI in healthcare is huge. Clinicians won't trust a black-box system. Did your master's thesis touch on that specifically?

[U-015] **User:** It's related. Symbolic AI emphasises on strict rules and we also study about knowledge graph and Rag system.

[F-015] **Feedback:**
- "emphasises on strict rules" → "emphasises strict rules" (no "on" with "emphasise")
- "study about knowledge graph" → "studied knowledge graphs" (past tense; drop "about"; plural)
- "Rag system" → "RAG systems" (acronym = all caps; plural)

[C-015] **Corrected:** "It's related. Symbolic AI emphasises strict rules, and we also studied knowledge graphs and RAG systems."

---

[S-016] **Marcus:** Oh nice — so you've got the neuro-symbolic angle and RAG on top of that. That's a pretty rare combo. Did you find they complemented each other well in your research?

[U-016] **User:** I think that's my job. Because they are expert in symbolic AI and knowledge graph but don't knlw much about the latest tech. I learned classic method from UCL and now I will integrate them. I think it's possible.

[F-016] **Feedback:**
- "they are expert in" → "they are experts in" (plural noun)
- "knowledge graph" → "knowledge graphs" (plural — recurring pattern)
- "knlw" → "know" (typo)
- "classic method from UCL" → "the classic methods at UCL" (plural; article "the"; "at" not "from")
- "now I will integrate them" → "now I want to integrate them" (softer, more natural in casual speech)

[C-016] **Corrected:** "I think that's my role. Because they are experts in symbolic AI and knowledge graphs but don't know much about the latest tech. I learned the classic methods at UCL, and now I want to integrate them. I think it's possible."

---

[S-017] **Marcus:** *leans forward* That's actually a really interesting position to be in. Bridging old-school knowledge engineering with modern LLMs — that gap is real. Are you thinking transformer-based grounding, or something more like a hybrid reasoning layer on top?

[U-017] **User:** I want to dig on weighted knowledge graph updating weight by a backpropagation to make sure final reasoning is visible.

[F-017] **Feedback:**
- "dig on" → "dig into" (preposition)
- "updating weight" → "updating weights" (plural)
- "by a backpropagation" → "via backpropagation" (no article before technique name)
- "make sure final reasoning is visible" → "ensure the final reasoning is visible" (article "the" missing)

[C-017] **Corrected:** "I want to dig into weighted knowledge graphs, updating weights via backpropagation to ensure the final reasoning is visible."

---

[S-018] **Marcus:** Oh, interesting — so basically making the graph itself learnable, but keeping the reasoning path interpretable. Has anyone done that at scale in a clinical context that you know of? Explainability is the whole game in regulated healthcare.

[U-018] **User:** I'm not sure. There are some articles about differential knowledge graph but I haven't check thoroughly. *(food arrives)* Oh, looks nice!

[F-018] **Feedback:**
- "differential knowledge graph" → "differentiable knowledge graphs" ("differentiable" is the correct ML term; plural)
- "I haven't check thoroughly" → "I haven't checked thoroughly" (past participle)

[C-018] **Corrected:** "I'm not sure. There are some articles about differentiable knowledge graphs, but I haven't checked thoroughly."

---

[S-019] **Marcus:** Fair enough — definitely worth digging into. *food arrives* Oh, perfect timing!
