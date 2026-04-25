# S2 — Station Saturation / WMS Picking Delays (STAR Versions)

Story base: SSG.COM WMS optimisation layer. Took ownership of third-party legacy code that nobody wanted to touch, fixed it at code-level, then went one layer deeper to fix the structural inventory distribution.

Themes: ownership, legacy code, root-cause analysis, floor-user collaboration.

Mapped questions (in `interview_qa_all.md`):
- Q20 — Walk me through the station saturation problem. (Technical)

Bonus framings this story fits:
- Tell me about taking ownership of something nobody wanted to touch.
- Tell me about a time you did deep root-cause analysis.
- Tell me about a debugging story.
- Tell me about working with operations / end users on the floor.

When NOT to use S2: trade-off / multi-objective / stakeholder reframing questions. Use S1 for those.

---

## Story Beats (memorise these, not sentences)

1. Picking work kept getting delayed. Some days we cut off orders early.
2. Optimisation layer was third-party vendor code. Spaghetti. Nobody wanted to touch it.
3. I took it on. Reverse-engineered. It wasn't checking inventory properly.
4. Didn't trust code alone. Went to the floor.
5. Contractors hit stockouts. But inventory was usually there. They only checked the nearest shelf, not boxes behind.
6. Added two code-level changes: checkpoint-based station redirection (overflow prevention) + cooldown for stations with recent stockouts.
7. But the real root cause was structural. Pulled inventory distribution and hit rates across stations.
8. High-velocity items concentrated in some stations, cold stock idle in others.
9. Shared with floor manager. Redistributed physically.
10. Saturation stopped recurring under normal volume.
11. Learning: code-level fix necessary but not sufficient. Triangulate code + floor + data.

---

## Version A — Q20 (Technical: "Walk me through the station saturation problem")

**[Headline]**
"Sure. The case was station saturation in our WMS. Picking work was getting delayed and we sometimes had to cut off orders early."

**[S+T]**
"The optimisation layer was written by a third-party vendor while I was on another project. The code was spaghetti. Nobody on the team wanted to touch it. Fortunately I like spaghetti, so I took it on."

**[Action]**
"When I dug into the code, I found it wasn't checking inventory properly. But I didn't trust code analysis alone, so I went to the floor. Contractors were running into stockouts. The strange part was the inventory was usually there. They were only checking the nearest shelf, not the boxes behind it."

"I introduced two changes. First, at each checkpoint in the picking process, the system compares current inventory against expected task demand. If a station is going to overflow, it redirects to another station. Second, I added a configurable cooldown. If a station had stockouts in the last N minutes, it gets redirected too."

"But I thought the real root cause was structural. I pulled inventory distribution and hit rates across stations. Some stations had high-velocity items concentrated, others had cold stock sitting idle. I shared the findings with the floor manager and we redistributed the inventory physically."

**[Result]**
"After the redistribution, we stopped hitting saturation under normal volume. The structural fix held. Happy to go deeper on the redirection logic or the inventory analysis if useful."

**Key signals:** technical chronology, light wit ("I like spaghetti"), enabler closer.

---

## Version B — Ownership ("Took on something nobody wanted to touch")

**[Headline]**
"One example. At SSG.COM, our optimisation layer was third-party vendor code. The code was spaghetti. Nobody wanted to touch it. I took it on, because the system was causing real damage."

**[S+T]**
"Picking work kept getting delayed and some days we cut off orders early. So fixing it mattered."

**[Action]**
"First I reverse-engineered the code. It wasn't checking inventory properly. But I didn't stop there. I went to the floor to verify. Contractors were running into stockouts. The strange part was the inventory was usually there. They were only checking the nearest shelf, not the boxes behind it."

"I introduced two changes. At each checkpoint, the system compares current inventory against expected demand. If a station is going to overflow, it redirects. And if a station had stockouts in the last N minutes, that triggers a redirect too."

"But the real root cause was structural. I analysed inventory distribution and hit rates across stations. Some stations had high-velocity items concentrated, others had cold stock idle. I shared the findings with the floor manager and we redistributed physically."

**[Result]**
"Saturation stopped recurring under normal volume. The lesson for me was that ownership doesn't end when you've patched the symptom. If something was broken enough that nobody wanted to touch it, you keep digging until the system actually works."

**Key signals:** "took it on", "I didn't stop there", "ownership doesn't end when you've patched the symptom".

---

## Version C — Root Cause ("Deep root-cause analysis")

**[Headline]**
"One example. At SSG.COM I had a problem where the obvious code-level fix was real, but it wasn't actually solving the system. I had to peel back three layers."

**[S+T]**
"Picking work kept getting delayed in our warehouse. The optimisation layer was third-party vendor code. Spaghetti. Nobody wanted to touch it, so I did."

**[Action]**
"**First layer.** I dug into the code and found it wasn't checking inventory properly. I could have stopped there with a code fix."

"**Second layer.** I went to the floor. Contractors were running into stockouts. The inventory was usually there. They were only checking the nearest shelf, not the boxes behind it. So I added two pieces of logic. Checkpoint-based station redirection when inventory wouldn't cover demand. And a cooldown for stations with recent stockouts."

"**Third layer.** Even with both fixes, saturation could recur. I pulled inventory distribution and hit rates across stations. Some had high-velocity items concentrated, others had cold stock sitting idle. The structural placement was the actual cause. I shared with the floor manager and we redistributed physically."

**[Result]**
"After the redistribution, saturation stopped recurring under normal volume. The takeaway for me was that root-cause analysis isn't one step. Each fix exposes the next layer. The code fix was necessary, but not sufficient."

**Key signals:** explicit "first/second/third layer" structure, principle in closing.

---

## Version D — Debugging ("Walk me through a debugging story")

**[Headline]**
"One I think about often. The bug was in the code, but it was also on the warehouse floor. I had to triangulate between both to actually fix it."

**[S+T]**
"At SSG.COM, picking work kept getting delayed and we cut off orders early some days. The optimisation logic was third-party vendor code. Spaghetti. Nobody wanted to touch it, so I took it on."

**[Action]**
"Step one was reading the code. It wasn't checking inventory properly. But that didn't fully explain the pattern, so I went to the floor."

"Watching the contractors work showed me the missing piece. They were processing stockouts when items weren't there. But the items were usually there. They were just checking the nearest shelf and not looking behind. That was a real-world signal the code couldn't see."

"I added two checks. At each checkpoint, the system compares inventory against expected demand and redirects if it's going to overflow. A station with recent stockouts also gets redirected."

"Then I went one more layer. Inventory distribution across stations was uneven. High-velocity items concentrated in some, cold stock in others. The floor manager and I redistributed it physically."

**[Result]**
"After the redistribution, we stopped hitting saturation under normal volume. The lesson for me was that debugging often means triangulating between what the code says, what the floor shows, and what the data reveals. Any one of those alone would have left the problem half-fixed."

**Key signals:** "triangulate", code + floor + data trio in closing.

---

## Quick comparison

| Version | Headline focus | Closing focus |
|---------|---------------|--------------|
| A — Q20 | Technical setup, light wit | "Want me to go deeper?" |
| B — Ownership | "Took it on, system was causing damage" | "Ownership doesn't end at the symptom" |
| C — Root cause | "Three layers" | "Each fix exposes the next layer" |
| D — Debugging | "Code + floor triangulation" | Code + floor + data trio |

---

## Practice notes

- Memorise the 11 beats above, not the sentences.
- "I like spaghetti" line is intentional levity. Keep it in A and optionally B; drop in C and D where the analytical tone is heavier.
- No quantitative metrics. Throughput depends on order volume and workforce, so a clean number isn't fair. Stick to "stopped hitting saturation under normal volume".
- 30s compressed version: "Took on third-party legacy code that nobody wanted. Code wasn't checking inventory. Went to the floor and saw contractors only checked the nearest shelf. Added two redirection rules in code. But the real cause was uneven inventory placement across stations, so we redistributed physically. After that, saturation stopped recurring."
