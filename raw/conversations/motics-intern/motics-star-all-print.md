# Motics Interview — STAR Versions (Print Edition)

Compiled: 2026-04-25

---

## Table of Contents

- **S1** — AutoStore Multi-Objective Optimisation
  - Version A: Q21 (Technical)
  - Version B: Q40 (Stakeholder)
  - Version C: Q24 (Process)
  - Version D: Challenge Q (Principle)
- **S2** — Station Saturation
  - Version A: Q20 (Technical)
  - Version B: Ownership
  - Version C: Root Cause
  - Version D: Debugging
- **S3** — Vehicle Resource Optimisation
  - Sub-story A: Q18 (Rule-based vs ML)
  - Sub-story B: Q12 (Ambiguity)
  - Sub-story C: Q23 (Pushback)
- **S4** — React Migration + Lecture Series
  - Sub-story A: Q39 (Migration + Lectures)
  - Sub-story B: Q41 (Touch Grid + Re-render)
  - Sub-story C: Q10 (Mentoring)
- **S5** — Pitwall / CogEn
  - Version A: Q13 (CogEn Walkthrough)
  - Version B: Q14 (Belief Change Deep-Dive)
  - Version C: Q16 (Pitwall + Safety)
  - Version D: Q22 (Production / STAR)

---
---

# S1 — AutoStore Multi-Objective Optimisation

**Themes:** ownership · problem redefinition · stakeholder alignment · trade-off design

**Story core:** SSG.COM AutoStore expiry fix. Owned the warehouse optimisation layer end-to-end.

**Beats:** (1) expired item came out, ops said "stop that" (2) item was fast-moving — didn't add up — traced bin selection (3) logic was picking most-stock first (4) history: original was expiry-first, caused throughput delays, switched (5) simple revert would bring back old problem — reframed with business team (6) real goal: get near-expiry stock consumed in time (7) hybrid: expiry-sorted + bins covering daily volume get equal priority + manual mode switch (8) stakeholders asked for automation → added auto-switch layer (9) both objectives held (10) literal request hides real problem.

---

## S1 / Version A — Q21 (Technical)

> "Sure. The case was AutoStore bin selection, where I had to balance expiry safety against picking throughput."
>
> "Background. AutoStore stores items in bins stacked in a grid. The existing logic picked bins with the most stock, which optimised for throughput. The catch was that newly replenished bins always had the highest count, so older stock often sat until it expired. One day an expired item came out during picking."
>
> "I traced the bin selection and confirmed it was the cause. I asked the team about the history. The original logic was expiry-first, but pickers had to wait on a single bin until empty, which caused delays. So they switched."
>
> "I designed a hybrid. First, sort all bins by expiry. Then calculate expected daily consumption, and give all bins needed to cover that demand equal priority. Within a day the order isn't strictly expiry-first. But by end of day, older stock gets consumed. That preserves throughput while preventing expiry."
>
> "I added a mode switch for high-volume periods. When workload and remaining time hit a threshold, the system flips to throughput-first."
>
> "Stakeholders asked if the switch could be automated, so I added that as an additional layer. Both objectives held in production. I can go deeper on the threshold calculation or the trade-off if useful."

---

## S1 / Version B — Q40 (Stakeholder)

> "At SSG.COM I owned the WMS, so I worked with floor managers and contractors regularly. The most useful thing I learned from them was that the literal request usually isn't the actual goal. One example."
>
> "Operations told me an expired product came out of our AutoStore during picking. The ask was simple. Stop that from happening."
>
> "I could have just patched the validation. But I checked the expired item first. It was a fast-moving product, which felt off. I traced the bin logic and found it was picking bins with the most stock. I asked colleagues why. Turned out the original logic was expiry-first, but it caused throughput delays, so they switched."
>
> "I went back to the business team. Not to confirm the fix, but to reframe the problem. We talked it through and the real objective came out. It wasn't blocking expired items. It was making sure near-expiry stock got consumed in time."
>
> "With that, I designed a hybrid. Bins sorted by expiry, but bins covering daily volume got equal priority. Plus a mode switch for high-volume periods."
>
> "When I shared the design, stakeholders pushed it further and asked if the switch could be automated. I added that as another layer."
>
> "Both objectives held. The lesson I took from working with floor teams was simple. Always ask what the real goal is before solving what's on the request."

---

## S1 / Version C — Q24 (Process)

> "My process is roughly: clarify the goal, check feasibility and side effects, design, share, iterate. Let me walk through it with one example."
>
> "At SSG.COM, an expired product came out of our AutoStore during picking. Operations asked me to make sure it didn't happen again. I owned the warehouse optimisation, so this was on me."
>
> "**First, clarify.** The literal ask was to block expired items. Before designing anything, I checked the item itself. It was a fast-moving product, which suggested validation wasn't the issue. I traced the bin selection logic and found it was picking bins with the most stock."
>
> "**Second, history and feasibility.** I asked the team. The original logic was expiry-first, but it caused throughput delays. So a simple revert wasn't an option."
>
> "**Third, reframe with the requester.** I went back to the business team. The real goal wasn't blocking expired items. It was getting near-expiry stock consumed in time."
>
> "**Fourth, design.** Bins sorted by expiry first, but bins covering daily volume got equal priority. Mode switch for high-volume periods."
>
> "**Fifth, share and iterate.** Stakeholders agreed and asked if the switch could be automated. I built that in as an additional layer."
>
> "Both objectives held. That's the process. Each step has a question to answer. What's the actual goal. What's feasible. What's the design. What did I miss."

---

## S1 / Version D — Challenge Q (Principle)

> "When I face a challenge, my first principle is not to trust what's in front of me. The literal request usually hides the real problem. Let me give you an example."
>
> "At SSG.COM, an expired product came out of our AutoStore during picking. The ask was straightforward. Stop that from happening."
>
> "The easy fix would have been to look at the validation logic. But I checked the expired item itself first. It was a fast-moving product. That didn't make sense, so I went one layer deeper. I traced the bin selection and found it was picking bins with the most stock."
>
> "I asked the team about the history. The original logic was expiry-first, but it caused throughput delays. So a revert would just bring back the old problem. That meant the solution had to address both objectives."
>
> "I went back to the business team and the real goal came out. It wasn't blocking expired items. It was getting near-expiry stock consumed in time. That reframing changed the design entirely."
>
> "I built a hybrid. Bins sorted by expiry, with bins covering daily volume getting equal priority. Plus a mode switch for high-volume periods. Stakeholders asked if that could be automated, so I added an automation layer."
>
> "Both objectives held. That's the pattern I follow. Find the root cause, understand what people actually need, design with side effects in mind."

---
---

# S2 — Station Saturation / WMS Picking Delays

**Themes:** ownership · legacy code · root-cause analysis · floor-user collaboration

**Story core:** SSG.COM WMS optimisation layer. Took ownership of third-party legacy code that nobody wanted to touch. Fixed at code level, then went one layer deeper to fix structural inventory distribution.

**Beats:** (1) picking delayed, orders cut off early some days (2) optimisation layer was third-party vendor code, spaghetti, nobody wanted to touch (3) took it on, reverse-engineered, inventory check broken (4) didn't trust code alone, went to floor (5) contractors hit stockouts but inventory was usually there — they only checked nearest shelf (6) added two code changes: checkpoint-based redirect + cooldown (7) but real cause was structural — pulled inventory distribution + hit rates (8) high-velocity items concentrated, cold stock idle elsewhere (9) shared with floor manager, redistributed physically (10) saturation stopped recurring under normal volume (11) code-level fix necessary but not sufficient.

---

## S2 / Version A — Q20 (Technical)

> "Sure. The case was station saturation in our WMS. Picking work was getting delayed and we sometimes had to cut off orders early."
>
> "The optimisation layer was written by a third-party vendor while I was on another project. The code was spaghetti. Nobody on the team wanted to touch it. Fortunately I like spaghetti, so I took it on."
>
> "When I dug into the code, I found it wasn't checking inventory properly. But I didn't trust code analysis alone, so I went to the floor. Contractors were running into stockouts. The strange part was the inventory was usually there. They were only checking the nearest shelf, not the boxes behind it."
>
> "I introduced two changes. First, at each checkpoint in the picking process, the system compares current inventory against expected task demand. If a station is going to overflow, it redirects to another station. Second, I added a configurable cooldown. If a station had stockouts in the last N minutes, it gets redirected too."
>
> "But I thought the real root cause was structural. I pulled inventory distribution and hit rates across stations. Some stations had high-velocity items concentrated, others had cold stock sitting idle. I shared the findings with the floor manager and we redistributed the inventory physically."
>
> "After the redistribution, we stopped hitting saturation under normal volume. The structural fix held. Happy to go deeper on the redirection logic or the inventory analysis if useful."

---

## S2 / Version B — Ownership

> "One example. At SSG.COM, our optimisation layer was third-party vendor code. The code was spaghetti. Nobody wanted to touch it. I took it on, because the system was causing real damage."
>
> "Picking work kept getting delayed and some days we cut off orders early. So fixing it mattered."
>
> "First I reverse-engineered the code. It wasn't checking inventory properly. But I didn't stop there. I went to the floor to verify. Contractors were running into stockouts. The strange part was the inventory was usually there. They were only checking the nearest shelf, not the boxes behind it."
>
> "I introduced two changes. At each checkpoint, the system compares current inventory against expected demand. If a station is going to overflow, it redirects. And if a station had stockouts in the last N minutes, that triggers a redirect too."
>
> "But the real root cause was structural. I analysed inventory distribution and hit rates across stations. Some stations had high-velocity items concentrated, others had cold stock idle. I shared the findings with the floor manager and we redistributed physically."
>
> "Saturation stopped recurring under normal volume. The lesson for me was that ownership doesn't end when you've patched the symptom. If something was broken enough that nobody wanted to touch it, you keep digging until the system actually works."

---

## S2 / Version C — Root Cause

> "One example. At SSG.COM I had a problem where the obvious code-level fix was real, but it wasn't actually solving the system. I had to peel back three layers."
>
> "Picking work kept getting delayed in our warehouse. The optimisation layer was third-party vendor code. Spaghetti. Nobody wanted to touch it, so I did."
>
> "**First layer.** I dug into the code and found it wasn't checking inventory properly. I could have stopped there with a code fix."
>
> "**Second layer.** I went to the floor. Contractors were running into stockouts. The inventory was usually there. They were only checking the nearest shelf, not the boxes behind it. So I added two pieces of logic. Checkpoint-based station redirection when inventory wouldn't cover demand. And a cooldown for stations with recent stockouts."
>
> "**Third layer.** Even with both fixes, saturation could recur. I pulled inventory distribution and hit rates across stations. Some had high-velocity items concentrated, others had cold stock sitting idle. The structural placement was the actual cause. I shared with the floor manager and we redistributed physically."
>
> "After the redistribution, saturation stopped recurring under normal volume. The takeaway for me was that root-cause analysis isn't one step. Each fix exposes the next layer. The code fix was necessary, but not sufficient."

---

## S2 / Version D — Debugging

> "One I think about often. The bug was in the code, but it was also on the warehouse floor. I had to triangulate between both to actually fix it."
>
> "At SSG.COM, picking work kept getting delayed and we cut off orders early some days. The optimisation logic was third-party vendor code. Spaghetti. Nobody wanted to touch it, so I took it on."
>
> "Step one was reading the code. It wasn't checking inventory properly. But that didn't fully explain the pattern, so I went to the floor."
>
> "Watching the contractors work showed me the missing piece. They were processing stockouts when items weren't there. But the items were usually there. They were just checking the nearest shelf and not looking behind. That was a real-world signal the code couldn't see."
>
> "I added two checks. At each checkpoint, the system compares inventory against expected demand and redirects if it's going to overflow. A station with recent stockouts also gets redirected."
>
> "Then I went one more layer. Inventory distribution across stations was uneven. High-velocity items concentrated in some, cold stock in others. The floor manager and I redistributed it physically."
>
> "After the redistribution, we stopped hitting saturation under normal volume. The lesson for me was that debugging often means triangulating between what the code says, what the floor shows, and what the data reveals. Any one of those alone would have left the problem half-fixed."

---
---

# S3 — Vehicle Resource Optimisation (Three Sub-Stories)

**Themes:** ambiguity · trade-off · pushback on stakeholder request · cross-functional comms

**Project context:** SSG.COM Vehicle Resource Optimisation. Led the engineering. Three distinct sub-stories within the same project, each fitting a different question type.

---

## S3 / Sub-story A — Q18 (Rule-based vs ML)

> "One technical judgment I'd point to is from the Vehicle Resource Optimisation project. We chose a rule-based approach over machine learning, even though forecasting was the core of the system."
>
> "The project predicted order volumes per centre and calculated how many vehicles each centre needed. I led the engineering. ML was the obvious default for forecasting."
>
> "Before committing to ML, I looked at the data conditions. The prediction targets weren't stable. Each centre and store didn't have a fixed postcode assignment. It changed depending on operations. Store holidays shifted too. Whenever that happened, the patterns an ML model would learn would stop working. Fixing it would have required significant data engineering, continuous retraining, plus drift detection."
>
> "So I designed a rule-based forecaster instead. It uses two years of historical data and the previous month. I calculate ratios by month, by week, and by day of week, then apply those to last month's actuals to predict next month. Simple, but it absorbs the operational shifts because the rules don't depend on stable categorical mappings."
>
> "We hit around 95% prediction accuracy. The system drove a 10.5% increase in per-vehicle throughput and became the SOP across all centres. The takeaway for me was that the right choice depends on data conditions, not on which approach is more sophisticated."

---

## S3 / Sub-story B — Q12 (Ambiguity)

> "One example of working through ambiguity. I led the Vehicle Resource Optimisation project at SSG.COM. The brief was three words. Make it automatic."
>
> "Operational managers were scheduling vehicles entirely on Excel and personal experience. They wanted automation, but no one could tell me what 'automatic' meant in practice. The data needed to automate fleet sizing didn't exist. And each fulfilment centre had its own undocumented constraints that nobody had written down."
>
> "Before writing a line of code, I ran structured interviews with managers at every centre. The goal was to surface the hidden rules in their heads. Things like minimum duty hours per driver, fleet scaling boundaries, dispatch slot constraints. Each centre had its own version."
>
> "What I realised was that hard-coding those rules would break the moment any centre changed something. So I built a configurable criteria management interface. Each centre could define and control its own operational rules. The system just consumes whatever rules they set. Tribal knowledge became something the system could act on, but the centres still owned it."
>
> "The system drove a 10.5% increase in per-vehicle throughput and a 4x reduction in admin overhead. It became the SOP across all centres. The takeaway was that ambiguity isn't always solved by more specification. Sometimes you have to give the user the tools to specify it themselves."

---

## S3 / Sub-story C — Q23 / Q-B3 (Pushback on Senior Stakeholder)

> "One example of pushing back on a senior stakeholder. At SSG.COM, a part lead from the business team asked us to add marketing data into our order volume forecasts. He was senior to me, and from a different team. So saying no needed to be careful and grounded."
>
> "I was building the forecasting layer for the Vehicle Resource Optimisation system. Marketing felt like a natural signal for predicting demand, so the request seemed reasonable on the surface."
>
> "Before disagreeing, I tried to make it work. I investigated every available data source and went directly to the marketing team to understand how their data was structured. Two problems came out."
>
> "First, a timing issue. We received prediction data four weeks before execution. Marketing campaigns weren't decided that early. The data simply wouldn't exist when the model needed it."
>
> "Second, a granularity issue. Our predictions were at individual store level. Marketing spend was applied much more broadly. Mapping the impact accurately wasn't feasible without restructuring how the entire business recorded spend."
>
> "To make sure I wasn't being conservative, I ran a regression with the data we did have. The p-value wasn't significant. Even with what existed, the signal wasn't usable."
>
> "I went back to the part lead and walked him through both structural problems with the regression result. I framed it around the shared goal — predicting demand accurately mattered — and separated that from the proposed solution. The numbers weren't opinion. He didn't love hearing it, but it was concrete."
>
> "He agreed to defer. The project shipped without marketing data and delivered on its targets. Looking back, that was the implicit validation. The signal we were debating about adding genuinely wasn't needed. The takeaway for me was that pushing back on someone senior works when you separate the goal from the solution, bring data instead of opinion, and show you genuinely tried to make their idea work first."

---
---

# S4 — React Migration + Lecture Series (Three Sub-Stories)

**Themes:** mentoring · learning-curve leadership · technical choice under uncertainty

**Project context:** SSG.COM React migration. Led the technical case, executed migration, built adoption layer (lecture series). Touch Grid is a separate technical sub-story within the same React work.

---

## S4 / Sub-story A — Q39 (Migration + Lecture Series)

> "One example of leading a tech transition at SSG.COM. Our team was all backend engineers. We were using JSP for frontend. I led the migration to React and built the adoption layer around it."
>
> "JSP made every UI change slow. CSS changes had to go through a designer and a separate web development team. Component reuse was barely practical. I led the technical case for React and the team approved the migration."
>
> "The migration itself was the easier half. The harder part was adoption. React was new to everyone on the team. Not many people studied it independently. Most questions came to me."
>
> "I noticed the same conceptual gaps coming up over and over. Why hooks behave the way they do. Why re-renders happen. Why state has to be lifted in certain ways. So instead of answering one-off questions forever, I recorded a series of lectures on React fundamentals and published them internally."
>
> "I also paired Chakra UI with the migration so styling decisions could happen at the engineer level. CSS no longer needed to round-trip through design and web teams."
>
> "Development cycles shortened. Component reuse became practical. One colleague told me the lectures were what finally made the React structure click for him. The takeaway for me was that adopting a new technology is not the same as introducing it. The technical migration is the smaller half. The bigger half is making sure the team can actually use it."

---

## S4 / Sub-story B — Q41 (Touch Grid + Re-render)

> "One technical challenge I worked through in our React frontend was around Touch Grid. It's a core UI component used across many warehouse tasks. Two problems came up: layout flexibility and re-render stability."
>
> "Each task type wanted a different layout. New design requests came in constantly. And because the screens hold in-progress data, full re-renders were a real cost. Scroll position resets, partial input lost."
>
> "For layout. Rather than building a new component for every variation, I defined a set of standard types that teams could configure. I also let the layout definition accept Component, logic, and event handlers directly. That gave teams full control without me being in the loop for every new variation."
>
> "For re-renders. I designed the hooks and state update flow to minimise unnecessary re-renders by default. For state that had to persist across renders, I used Context API to keep it stable independently of the parent's render cycle. So even when something upstream changed, the in-progress data stayed."
>
> "The component absorbed dozens of layout variants without forking the codebase, and operator-facing screens stayed stable through state changes. The takeaway was that the right abstraction at the right layer saves a lot of downstream pain."

---

## S4 / Sub-story C — Q10 (Mentoring)

> "One example of upskilling people. At SSG.COM, I led the React migration on our team. The whole team was backend engineers. React was new to everyone."
>
> "Most questions came to me. Some I could answer in a sentence. But the same conceptual gaps kept coming back. Hooks. Re-renders. Why state has to be lifted in certain ways."
>
> "I realised one-off answers weren't going to scale. People needed the fundamentals laid out cleanly, not patched in over Slack."
>
> "So I recorded a series of lectures on React fundamentals and published them internally. I structured them around the conceptual gaps I'd seen most often, not a generic curriculum. People could go through them at their own pace, then come back with sharper questions."
>
> "On top of that, I paired with people directly when they hit something concrete. The lectures handled the foundation. The pairing handled the application."
>
> "One colleague told me the lectures were what finally made the React structure click for him. The team moved from depending on me for answers to writing React on their own. The lesson for me was that mentoring at scale isn't about being available. It's about removing yourself as the bottleneck."

---
---

# S5 — Pitwall / CogEn (Project Walkthroughs + Production)

**Note:** S5 is structurally different from S1–S4. Three of the four are technical project walkthroughs; only Q22 fits a STAR-style answer naturally.

---

## S5 / Version A — Q13 (CogEn Walkthrough)

> "CogEn is a conversational system I'm building around belief change modeling. The core idea is simple. Most AI systems treat knowledge as static. I wanted to build something that reflects reality. Beliefs change over time."
>
> "It works in three steps. When you say something, the system transforms your input into a structured format. It then retrieves relevant context from a knowledge graph using both vector embedding search and node traversal. The system generates a response from that. After responding, the knowledge graph updates asynchronously. Existing beliefs get reinforced, weakened, or terminated based on what you said."
>
> "Why this matters for Motics. Your Scribe Agent already extracts symptoms and duration from conversations. If those are structured into a knowledge graph, ICD suggestion becomes a graph search problem. Combined with patient history and medical knowledge, the system can make more specific decisions. The graph also enforces hard clinical rules. What must be present. What cannot. Those are cases a probabilistic model cannot reliably catch."
>
> "I read that your Scribe Agent has reached 90% accuracy in ICD coding. The remaining 10% is where a knowledge graph can help."

---

## S5 / Version B — Q14 (Belief Change Deep-Dive)

> "When a message comes in, CogEn structures the input the same way data is stored in the graph. Then it runs both embedding-based search and node traversal to pull the relevant belief states. I'm also adding graph structure embedding to make retrieval more precise."
>
> "Based on what's retrieved, the graph updates. New knowledge gets created. Existing beliefs get reinforced, weakened, or terminated. The LLM makes those judgments."
>
> "But I've worked to minimise how much depends on LLM judgment. Just passing information to the LLM and letting it decide is no different from RAG. LLM assistance at intermediate steps is fine. But the final reasoning should be explainable, traceable, and replicable. The last LLM call should only handle language."
>
> "I'm still deciding whether to build a service around this or release it as open source. So I hope you'll understand if I stop the detail there."

---

## S5 / Version C — Q16 (Pitwall + Safety)

> "Pitwall is a platform where twenty agents debate and vote on financial topics. Each agent has its own investment philosophy and accesses news, analyst target prices, and market data through dedicated MCP servers. Based on that data, each agent produces an argument, a target price, and a vote. At the topic deadline, the system evaluates all results."
>
> "On safety. The agent pipeline runs on LangGraph. When an agent produces output, the system validates it against a defined schema. If it fails, it retries. After a few retries, it stops and sends me a Telegram alert."
>
> "Schema-level validation is important. But I think healthcare needs more. Schema validation checks structure. It doesn't check whether the content is clinically valid. It means an output can pass schema validation and still be wrong."
>
> "That's where ontologies and knowledge graphs come in. They validate whether the relationships between clinical facts are consistent. So safe AI in healthcare needs both. Schema validation and ontology validation, both passing, before the output reaches a clinician."

---

## S5 / Version D — Q22 (Production / STAR-style)

> "The hardest part of applying AI to production is consistency. LLM systems produce variable output. Controlling that took significant effort. I'll walk through the three pieces that mattered most."
>
> "When I built Pitwall, I had twenty agents producing structured arguments, target prices, and votes. Any agent giving a malformed output broke the downstream evaluation. So consistency wasn't a nice-to-have. It was the central engineering risk."
>
> "**Prompt design.** Prompts need calibration. Too specific and the model gets trapped in the instruction. Too general and you lose control. Finding the right balance requires iteration. There's no shortcut. But if your test suite is concrete enough, an agent loop can handle much of that iteration for you."
>
> "**Context management.** When you give a model too much at once, performance drops and hallucination increases. The solution is decomposition. Decisions that can be made independently should be. You inject only what is relevant at each step. By the time the final decision is made, the model is working with refined, focused information."
>
> "**Structured output.** Rather than using the model's raw output directly, I define a template in advance. The model fills in pieces, validated against a schema, and the server assembles the final response by injecting those pieces into the template. The model never owns the shape of the output. The system does."
>
> "Pitwall is built exactly this way. The pipeline runs in four to five steps. Context not needed at a given step is stored separately and injected only when required. Every output is schema-checked before the next step runs. That's what made it reliable enough for production."

---
---

# Practice Reminders

- **Memorise beats, not sentences.** Vary the connecting language each time.
- **Inject thinking moments** ("So... when I checked the expired item..."). Avoid silky-smooth delivery — it sounds rehearsed.
- **Practise each version aloud 5+ times** with slight variation. Same beats, different phrasing.
- **Have a 30-second compressed version** ready in case the interviewer signals time pressure.
- **Offer follow-up depth** at the end of technical answers ("happy to go deeper on any part").
- **No em-dashes in spoken delivery** — use period + short sentences instead.
- **Read the room.** If the interviewer is nodding faster or checking the clock, jump to the result.
