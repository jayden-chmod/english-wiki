# S1 — AutoStore Multi-Objective Optimisation (STAR Versions)

Story base: SSG.COM AutoStore expiry fix. Owned the warehouse optimisation layer end-to-end.

Themes: ownership, problem redefinition, stakeholder alignment, trade-off design.

Mapped questions (in `interview_qa_all.md`):
- Q21 — Walk me through the multi-objective optimisation. (Technical)
- Q40 — Worked with operational teams or end users to define problems? (Stakeholder)
- Q24 — How would you architect a new feature end-to-end? (Process)
- Challenge Q — Tell me about a challenge you faced. (Principle)

Bonus questions this also covers:
- Tell me about a time you took ownership.
- Tell me about a trade-off you had to make.
- Tell me about a time you disagreed with a stakeholder request.
- Tell me about a hard technical problem.

---

## Story Beats (memorise these, not sentences)

1. Expired item came out of AutoStore. Ops said "stop that."
2. Item was fast-moving. That didn't add up. Traced bin selection logic.
3. Logic was picking bins with most stock. Asked team why.
4. History: original was expiry-first, caused throughput delays, switched to most-stock.
5. Simple revert would bring back the old problem. Reframed goal with business team.
6. Real goal: get near-expiry stock consumed in time, not just block expired items.
7. Designed hybrid: expiry-sorted bins + bins covering daily volume get equal priority + manual mode switch for high-volume periods.
8. Stakeholders asked for automation. Added auto-switch as another layer.
9. Both objectives held.
10. Learning: literal request hides the real problem.

---

## Version A — Q21 (Technical: "Walk me through the multi-objective optimisation")

**[Headline]**
"Sure. The case was AutoStore bin selection, where I had to balance expiry safety against picking throughput."

**[S+T]**
"Background. AutoStore stores items in bins stacked in a grid. The existing logic picked bins with the most stock, which optimised for throughput. The catch was that newly replenished bins always had the highest count, so older stock often sat until it expired. One day an expired item came out during picking."

**[Action]**
"I traced the bin selection and confirmed it was the cause. I asked the team about the history. The original logic was expiry-first, but pickers had to wait on a single bin until empty, which caused delays. So they switched."

"I designed a hybrid. First, sort all bins by expiry. Then calculate expected daily consumption, and give all bins needed to cover that demand equal priority. Within a day the order isn't strictly expiry-first. But by end of day, older stock gets consumed. That preserves throughput while preventing expiry."

"I added a mode switch for high-volume periods. When workload and remaining time hit a threshold, the system flips to throughput-first."

**[Result]**
"Stakeholders asked if the switch could be automated, so I added that as an additional layer. Both objectives held in production. I can go deeper on the threshold calculation or the trade-off if useful."

**Key signals:** algorithm detail; closes with an enabler ("want me to go deeper?").

---

## Version B — Q40 (Stakeholder: "Worked with operational teams to define problems")

**[Headline]**
"At SSG.COM I owned the WMS, so I worked with floor managers and contractors regularly. The most useful thing I learned from them was that the literal request usually isn't the actual goal. One example."

**[S+T]**
"Operations told me an expired product came out of our AutoStore during picking. The ask was simple. Stop that from happening."

**[Action]**
"I could have just patched the validation. But I checked the expired item first. It was a fast-moving product, which felt off. I traced the bin logic and found it was picking bins with the most stock. I asked colleagues why. Turned out the original logic was expiry-first, but it caused throughput delays, so they switched."

"I went back to the business team. Not to confirm the fix, but to reframe the problem. We talked it through and the real objective came out. It wasn't blocking expired items. It was making sure near-expiry stock got consumed in time."

"With that, I designed a hybrid. Bins sorted by expiry, but bins covering daily volume got equal priority. Plus a mode switch for high-volume periods."

"When I shared the design, stakeholders pushed it further and asked if the switch could be automated. I added that as another layer."

**[Result]**
"Both objectives held. The lesson I took from working with floor teams was simple. Always ask what the real goal is before solving what's on the request."

**Key signals:** "I went back to the business team", "reframe", "asked colleagues" — conversation-centric vocabulary.

---

## Version C — Q24 (Process: "Architect a new feature end-to-end")

**[Headline]**
"My process is roughly: clarify the goal, check feasibility and side effects, design, share, iterate. Let me walk through it with one example."

**[S+T]**
"At SSG.COM, an expired product came out of our AutoStore during picking. Operations asked me to make sure it didn't happen again. I owned the warehouse optimisation, so this was on me."

**[Action]**

"**First, clarify.** The literal ask was to block expired items. Before designing anything, I checked the item itself. It was a fast-moving product, which suggested validation wasn't the issue. I traced the bin selection logic and found it was picking bins with the most stock."

"**Second, history and feasibility.** I asked the team. The original logic was expiry-first, but it caused throughput delays. So a simple revert wasn't an option."

"**Third, reframe with the requester.** I went back to the business team. The real goal wasn't blocking expired items. It was getting near-expiry stock consumed in time."

"**Fourth, design.** Bins sorted by expiry first, but bins covering daily volume got equal priority. Mode switch for high-volume periods."

"**Fifth, share and iterate.** Stakeholders agreed and asked if the switch could be automated. I built that in as an additional layer."

**[Result]**
"Both objectives held. That's the process. Each step has a question to answer. What's the actual goal. What's feasible. What's the design. What did I miss."

**Key signals:** explicit First/Second/Third steps; closes by recapping the process.

---

## Version D — Challenge Q (Principle: "Tell me about a challenge you faced")

**[Headline]**
"When I face a challenge, my first principle is not to trust what's in front of me. The literal request usually hides the real problem. Let me give you an example."

**[S+T]**
"At SSG.COM, an expired product came out of our AutoStore during picking. The ask was straightforward. Stop that from happening."

**[Action]**
"The easy fix would have been to look at the validation logic. But I checked the expired item itself first. It was a fast-moving product. That didn't make sense, so I went one layer deeper. I traced the bin selection and found it was picking bins with the most stock."

"I asked the team about the history. The original logic was expiry-first, but it caused throughput delays. So a revert would just bring back the old problem. That meant the solution had to address both objectives."

"I went back to the business team and the real goal came out. It wasn't blocking expired items. It was getting near-expiry stock consumed in time. That reframing changed the design entirely."

"I built a hybrid. Bins sorted by expiry, with bins covering daily volume getting equal priority. Plus a mode switch for high-volume periods. Stakeholders asked if that could be automated, so I added an automation layer."

**[Result]**
"Both objectives held. That's the pattern I follow. Find the root cause, understand what people actually need, design with side effects in mind."

**Key signals:** Headline declares the principle; story illustrates it.

---

## Quick comparison

| Version | Headline focus | Closing focus |
|---------|---------------|--------------|
| A — Q21 | Technical setup ("balance expiry vs throughput") | "Want me to go deeper?" |
| B — Q40 | Cross-functional learning | "Always ask the real goal" |
| C — Q24 | Process steps declaration | Recap each step |
| D — Challenge | Problem-solving principle | Principle restated |

---

## Practice notes

- Memorise the 10 beats above, not the sentences.
- Practise each version 5+ times out loud, varying connectors each time.
- Inject thinking moments ("So... when I checked the expired item..."). Avoid silky-smooth delivery.
- Have a 30-second compressed version ready in case interviewer signals time pressure.
- After delivery, optionally offer "happy to go deeper on any part" to invite follow-up.
