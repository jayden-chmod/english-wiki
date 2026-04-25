# S4 — React Migration + Lecture Series (STAR Sub-Stories)

Story base: SSG.COM React migration. Led the technical case, executed migration, built adoption layer (lecture series). Touch Grid is a separate technical sub-story within the same React work.

Themes: mentoring · learning-curve leadership · technical choice under uncertainty.

Mapped questions (in `interview_qa_all.md`):
- Q39 — React productivity gains and challenges → Sub-story A (migration + lecture series)
- Q41 — Challenges working with React → Sub-story B (Touch Grid + re-render)
- Q10 — Mentoring or upskilling someone → Sub-story C (re-frame of A)

S4a and S4c share the same underlying story (lecture series). S4b is a distinct technical challenge story.

---

## Sub-story A — Q39 (React Migration + Lecture Series)

**Story beats**

1. Team was all backend engineers. JSP was the frontend.
2. JSP made every UI change slow. CSS round-tripped through designer + web team.
3. Led technical case for React. Migration approved.
4. Migration was the easier half. Adoption was the harder half.
5. Same conceptual gaps came up repeatedly: hooks, re-renders, state lifting.
6. Recorded a series of lectures on React fundamentals. Published internally.
7. Paired Chakra UI with migration so engineers could handle styling decisions.
8. Result: faster cycles, practical component reuse, colleague said lectures finally made it click.
9. Lesson: adopting a new technology is not the same as introducing it. Migration is the smaller half.

**Script**

**[Headline]**
"One example of leading a tech transition at SSG.COM. Our team was all backend engineers. We were using JSP for frontend. I led the migration to React and built the adoption layer around it."

**[S+T]**
"JSP made every UI change slow. CSS changes had to go through a designer and a separate web development team. Component reuse was barely practical. I led the technical case for React and the team approved the migration."

**[Action]**
"The migration itself was the easier half. The harder part was adoption. React was new to everyone on the team. Not many people studied it independently. Most questions came to me."

"I noticed the same conceptual gaps coming up over and over. Why hooks behave the way they do. Why re-renders happen. Why state has to be lifted in certain ways. So instead of answering one-off questions forever, I recorded a series of lectures on React fundamentals and published them internally."

"I also paired Chakra UI with the migration so styling decisions could happen at the engineer level. CSS no longer needed to round-trip through design and web teams."

**[Result]**
"Development cycles shortened. Component reuse became practical. One colleague told me the lectures were what finally made the React structure click for him. The takeaway for me was that adopting a new technology is not the same as introducing it. The technical migration is the smaller half. The bigger half is making sure the team can actually use it."

**Key signals:** "migration is the smaller half", colleague feedback as social proof.

---

## Sub-story B — Q41 (Touch Grid + Re-render / Technical Challenge)

**Story beats**

1. Touch Grid: core UI component used across many warehouse tasks.
2. Each task type wanted different layout. Constant new design requests.
3. Screens hold in-progress data, so full re-renders cost real damage (scroll resets, partial input lost).
4. Layout solution: defined standard configurable types. Layout definition accepts Component/logic/event handlers directly. Teams self-serve variations.
5. Re-render solution: minimised unnecessary re-renders by default. Used Context API for state that had to persist across renders independently of parent's render cycle.
6. Result: dozens of layout variants without forking; operator screens stable through state changes.
7. Lesson: right abstraction at the right layer saves downstream pain.

**Script**

**[Headline]**
"One technical challenge I worked through in our React frontend was around Touch Grid. It's a core UI component used across many warehouse tasks. Two problems came up: layout flexibility and re-render stability."

**[S+T]**
"Each task type wanted a different layout. New design requests came in constantly. And because the screens hold in-progress data, full re-renders were a real cost. Scroll position resets, partial input lost."

**[Action]**
"For layout. Rather than building a new component for every variation, I defined a set of standard types that teams could configure. I also let the layout definition accept Component, logic, and event handlers directly. That gave teams full control without me being in the loop for every new variation."

"For re-renders. I designed the hooks and state update flow to minimise unnecessary re-renders by default. For state that had to persist across renders, I used Context API to keep it stable independently of the parent's render cycle. So even when something upstream changed, the in-progress data stayed."

**[Result]**
"The component absorbed dozens of layout variants without forking the codebase, and operator-facing screens stayed stable through state changes. The takeaway was that the right abstraction at the right layer saves a lot of downstream pain."

**Key signals:** explicit two-problem split, abstraction-as-leverage lesson.

---

## Sub-story C — Q10 (Mentoring re-frame of A)

**Story beats**

1. Led React migration on team of backend engineers.
2. Most questions came to me. Same conceptual gaps recurring.
3. One-off answers don't scale.
4. Recorded lecture series, structured around real conceptual gaps (not generic curriculum).
5. Paired directly when people hit concrete problems. Lectures = foundation, pairing = application.
6. Colleague: lectures made React structure click for him.
7. Team moved from depending on me to writing React on their own.
8. Lesson: mentoring at scale isn't about being available. It's about removing yourself as the bottleneck.

**Script**

**[Headline]**
"One example of upskilling people. At SSG.COM, I led the React migration on our team. The whole team was backend engineers. React was new to everyone."

**[S+T]**
"Most questions came to me. Some I could answer in a sentence. But the same conceptual gaps kept coming back. Hooks. Re-renders. Why state has to be lifted in certain ways."

**[Action]**
"I realised one-off answers weren't going to scale. People needed the fundamentals laid out cleanly, not patched in over Slack."

"So I recorded a series of lectures on React fundamentals and published them internally. I structured them around the conceptual gaps I'd seen most often, not a generic curriculum. People could go through them at their own pace, then come back with sharper questions."

"On top of that, I paired with people directly when they hit something concrete. The lectures handled the foundation. The pairing handled the application."

**[Result]**
"One colleague told me the lectures were what finally made the React structure click for him. The team moved from depending on me for answers to writing React on their own. The lesson for me was that mentoring at scale isn't about being available. It's about removing yourself as the bottleneck."

**Key signals:** "lectures = foundation, pairing = application" structure, "removing yourself as the bottleneck" leadership signal.

**Note on closing line:** "Removing yourself as the bottleneck" reads as senior-engineer leadership language in English. In Korean it can sound slightly self-important; in English it lands as self-aware maturity. Keep it in spoken delivery.

---

## Quick comparison

| Sub-story | Question fit | Headline focus | Lesson |
|-----------|-------------|---------------|--------|
| A — Migration + lectures | Q39 | "Adoption layer around it" | Migration is the smaller half |
| B — Touch Grid + re-render | Q41 | "Layout flexibility + re-render stability" | Right abstraction at right layer |
| C — Mentoring re-frame | Q10 | "Backend engineers, React new to everyone" | Remove yourself as the bottleneck |

---

## Practice notes

- A and C share the same lecture-series anecdote. Don't tell both in the same interview without separating context.
- B is independent — usable even if A/C hasn't come up.
- A and C have no quantitative metrics; the social-proof line ("colleague said lectures made it click") is the credibility anchor. That's deliberate.
- 30s compressed version of A: "Led React migration from JSP. Migration was the easy half. Adoption was harder because the team was all backend engineers. Same conceptual gaps kept coming up, so I recorded a lecture series on React fundamentals. Paired Chakra UI alongside so engineers could own styling. Cycles shortened, reuse became practical, and a colleague told me the lectures finally made it click for him."
