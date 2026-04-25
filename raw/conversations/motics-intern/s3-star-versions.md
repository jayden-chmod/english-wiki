# S3 — Vehicle Resource Optimisation (STAR Sub-Stories)

Story base: SSG.COM Vehicle Resource Optimisation project. Led the engineering. Three distinct sub-stories within the same project, each fitting a different question type.

Themes: ambiguity · trade-off · pushback on stakeholder request · cross-functional comms.

Mapped questions (in `interview_qa_all.md`):
- Q18 — Vehicle Resource Optimisation deep-dive → Sub-story A (rule-based vs ML)
- Q12 — How do you handle ambiguity? → Sub-story B (tribal knowledge → criteria interface)
- Q23 — Explaining technical to non-technical / pushback → Sub-story C (marketing data refusal)

Each sub-story is shorter than S1/S2 (60–90s). Don't bundle them; each fits its own question type cleanly.

---

## Sub-story A — Q18 (Rule-based vs ML / Technical Judgment)

**Story beats**

1. VRO project: forecast order volume per centre, calculate vehicle count.
2. ML was the obvious default for forecasting.
3. Looked at data conditions first. Prediction targets weren't stable.
4. Postcode assignments per centre/store changed by situation. Store holidays shifted.
5. ML-learned patterns would break whenever those changed. Would need data engineering + retraining + drift detection.
6. Designed rule-based forecaster: two years history + previous month. Ratios by month/week/day-of-week applied to last month's actuals.
7. Result: ~95% accuracy, 10.5% per-vehicle throughput increase, became SOP.
8. Lesson: right choice depends on data conditions, not approach sophistication.

**Script**

**[Headline]**
"One technical judgment I'd point to is from the Vehicle Resource Optimisation project. We chose a rule-based approach over machine learning, even though forecasting was the core of the system."

**[S+T]**
"The project predicted order volumes per centre and calculated how many vehicles each centre needed. I led the engineering. ML was the obvious default for forecasting."

**[Action]**
"Before committing to ML, I looked at the data conditions. The prediction targets weren't stable. Each centre and store didn't have a fixed postcode assignment. It changed depending on operations. Store holidays shifted too. Whenever that happened, the patterns an ML model would learn would stop working. Fixing it would have required significant data engineering, continuous retraining, plus drift detection."

"So I designed a rule-based forecaster instead. It uses two years of historical data and the previous month. I calculate ratios by month, by week, and by day of week, then apply those to last month's actuals to predict next month. Simple, but it absorbs the operational shifts because the rules don't depend on stable categorical mappings."

**[Result]**
"We hit around 95% prediction accuracy. The system drove a 10.5% increase in per-vehicle throughput and became the SOP across all centres. The takeaway for me was that the right choice depends on data conditions, not on which approach is more sophisticated."

**Key signals:** "data conditions, not approach sophistication". Quantitative anchors (95%, 10.5%) ground credibility.

---

## Sub-story B — Q12 (Ambiguity / Tribal Knowledge)

**Story beats**

1. Brief was three words: "make it automatic."
2. Managers scheduled vehicles via Excel + personal experience.
3. Data didn't exist. Each centre had undocumented constraints.
4. Before any code, ran structured interviews with managers at every centre.
5. Surfaced hidden rules: duty hour minimums, fleet scaling boundaries, dispatch slot constraints.
6. Hard-coding would break on the next change. Built configurable criteria management interface.
7. Each centre defines and owns its own rules. System consumes whatever they set.
8. Result: 10.5% throughput, 4x admin reduction, SOP across centres.
9. Lesson: ambiguity isn't always solved by more specification. Give users tools to specify themselves.

**Script**

**[Headline]**
"One example of working through ambiguity. I led the Vehicle Resource Optimisation project at SSG.COM. The brief was three words. Make it automatic."

**[S+T]**
"Operational managers were scheduling vehicles entirely on Excel and personal experience. They wanted automation, but no one could tell me what 'automatic' meant in practice. The data needed to automate fleet sizing didn't exist. And each fulfilment centre had its own undocumented constraints that nobody had written down."

**[Action]**
"Before writing a line of code, I ran structured interviews with managers at every centre. The goal was to surface the hidden rules in their heads. Things like minimum duty hours per driver, fleet scaling boundaries, dispatch slot constraints. Each centre had its own version."

"What I realised was that hard-coding those rules would break the moment any centre changed something. So I built a configurable criteria management interface. Each centre could define and control its own operational rules. The system just consumes whatever rules they set. Tribal knowledge became something the system could act on, but the centres still owned it."

**[Result]**
"The system drove a 10.5% increase in per-vehicle throughput and a 4x reduction in admin overhead. It became the SOP across all centres. The takeaway was that ambiguity isn't always solved by more specification. Sometimes you have to give the user the tools to specify it themselves."

**Key signals:** "make it automatic" hook, "tools to specify it themselves" lesson.

---

## Sub-story C — Q23 / Q-B3 (Pushback on Senior Stakeholder)

**Story beats**

1. A part lead from the business team (senior, different team) asked to include marketing data in volume predictions.
2. Instinct says yes, more data should help. Tried to make it work first.
3. Investigated data sources. Went directly to marketing team.
4. Two structural problems: timing (data needed 4 weeks ahead, campaigns not decided yet) + granularity (predictions at store level, marketing spend much broader).
5. Ran regression with available data. P-value not significant.
6. Walked the part lead through both problems + regression result. Framed around shared goal, separated goal from proposed solution.
7. He didn't love hearing it, but it was concrete. Agreed to defer.
8. Project shipped without marketing data and delivered on targets — implicit validation.
9. Lesson: pushback on senior works when you separate goal from solution, bring data not opinion, and show you tried to make the request work first.

**Script**

**[Headline]**
"One example of pushing back on a senior stakeholder. At SSG.COM, a part lead from the business team asked us to add marketing data into our order volume forecasts. He was senior to me, and from a different team. So saying no needed to be careful and grounded."

**[S+T]**
"I was building the forecasting layer for the Vehicle Resource Optimisation system. Marketing felt like a natural signal for predicting demand, so the request seemed reasonable on the surface."

**[Action]**
"Before disagreeing, I tried to make it work. I investigated every available data source and went directly to the marketing team to understand how their data was structured. Two problems came out."

"First, a timing issue. We received prediction data four weeks before execution. Marketing campaigns weren't decided that early. The data simply wouldn't exist when the model needed it."

"Second, a granularity issue. Our predictions were at individual store level. Marketing spend was applied much more broadly. Mapping the impact accurately wasn't feasible without restructuring how the entire business recorded spend."

"To make sure I wasn't being conservative, I ran a regression with the data we did have. The p-value wasn't significant. Even with what existed, the signal wasn't usable."

"I went back to the part lead and walked him through both structural problems with the regression result. I framed it around the shared goal — predicting demand accurately mattered — and separated that from the proposed solution. The numbers weren't opinion. He didn't love hearing it, but it was concrete."

**[Result]**
"He agreed to defer. The project shipped without marketing data and delivered on its targets. Looking back, that was the implicit validation. The signal we were debating about adding genuinely wasn't needed. The takeaway for me was that pushing back on someone senior works when you separate the goal from the solution, bring data instead of opinion, and show you genuinely tried to make their idea work first."

**Key signals:** "He didn't love hearing it" (honest line, don't soften). "Tried to make it work first" (credibility anchor). Authority gradient explicit (part lead, different team, senior).

**Mapped questions:** Q23 (cross-functional comms), Q-B3 (disagreement with senior stakeholder). Use the same script for both — the headline already covers the senior dynamic.

---

## Quick comparison

| Sub-story | Question fit | Headline focus | Lesson |
|-----------|-------------|---------------|--------|
| A — Rule vs ML | Q18, technical trade-off | "Even though forecasting was the core" | Right choice = data conditions, not sophistication |
| B — Tribal knowledge | Q12, ambiguity | "Brief was three words. Make it automatic." | Give users tools to specify themselves |
| C — Marketing data | Q23, pushback | "Instinct says yes... but I checked first" | Pushback works when data-grounded |

---

## Practice notes

- These three are smaller than S1/S2 (60–90s each). Resist the urge to bundle them; each is sharp because it's focused.
- All three share VRO project context. If you've already established VRO earlier in the interview, you can drop the project-setup line and dive straight into the sub-story.
- All three carry quantitative anchors (10.5%, 4x, 95%). Keep them — they're earned numbers and ground the project credibly.
- For Q18, if interviewer probes deeper, follow-ups already drafted in qa_all (greedy algorithm, demand transfer, legacy integration).
