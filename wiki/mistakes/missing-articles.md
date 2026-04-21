---
title: "missing articles"
category: mistake
tags: [article, determiner, korean-l1-pattern]
occurrence_count: 5
last_occurred: 2026-04-21
severity: high
created: 2026-04-21
updated: 2026-04-21
---

# Missing Articles

## Pattern
Dropping `a` / `an` / `the` before a countable noun that needs a determiner. Korean has no article system, so this is a high-frequency L1-interference pattern — especially in fast, unrehearsed speech.

## Occurrences
| Date | What I wrote | Correct form | Source |
|------|-------------|-------------|--------|
| 2026-04-20 | it was natural decision | it was **a** natural decision | [[2026-04-20-motics-interview-session-03]] |
| 2026-04-21 | This is **a** generation screen | This is **the** generation screen | [[2026-04-21-motics-clindiff-walkthrough-practice]] |
| 2026-04-21 | set small model | set **the** small model | [[2026-04-21-motics-clindiff-walkthrough-practice]] |
| 2026-04-21 | from dropdowns | from **the** dropdowns | [[2026-04-21-motics-clindiff-walkthrough-practice]] |
| 2026-04-21 | hit generate button | hit **the** generate button / hit **Generate** | [[2026-04-21-motics-clindiff-walkthrough-practice]] |

## Pattern Breakdown

**1. Missing definite article before specific/unique items on screen**
When demonstrating a UI, every on-screen element is *specific* — use `the`:
- `the generate button`, `the dropdowns`, `the compare screen`, `the sidebar`

**2. Missing indefinite article before singular count nouns**
- "it was natural decision" → "it was **a** natural decision"
- "I'll pick small model" → "I'll pick **a** small model" (if talking generally) or "**the** small model" (if specific)

**3. Exception: UI labels as proper nouns**
When you read a button label aloud, capitalising it treats it as a proper noun — no article:
- "I hit **Generate**" ✓
- "I click **Save**" ✓

## Why This Is High Signal
- 4 occurrences in 3 short utterances during live demo narration — compressed into one session.
- Happens most under time pressure or when narrating visual content ("this is X", "set X", "click X").
- English listeners parse articles subconsciously; missing articles create a consistent "non-native" signal even when content is strong.

## Mitigation Strategies
1. **Pre-script UI narration**: for each screen in the clindiff walkthrough, write out "this is **the** X / click **the** Y / navigate to **the** Z" once, read it aloud.
2. **Capitalise-or-article rule for buttons**: either say "Generate" (label/proper noun) or "the generate button" — never "generate button".
3. **Default to `the` for anything on-screen**: in demo mode, almost nothing is newly introduced — it's all visible, therefore specific, therefore `the`.

## Related
- [[subject-verb-agreement-errors]] — another Korean-L1 pattern driven by the same absence-of-markers issue
