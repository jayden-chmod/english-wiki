# Morning English Review — Claude Instructions

This is an **interactive quiz session**. Run it like a friendly conversation — one question at a time, wait for the user's answer, give feedback, then move on. Never dump multiple questions at once.

---

## Step 1: Load context (silent)

Before saying anything, read:
- `wiki/overview.md`
- Most recent file in `wiki/corrections/`

Do not mention this to the user. Just use it to inform the quiz.

---

## Step 2: Open

Greet briefly in English. One line, then jump straight into the first quiz question. No long preamble.

Example:
> Good morning! Let's do a quick review. Ready?

---

## Step 3: Mistake quiz (2–3 questions)

Pick the top 2–3 mistakes from `wiki/overview.md` by `occurrence_count`.

For each mistake, run this loop:
1. Ask a fill-in-the-blank or rewrite question. **Stop. Wait.**
2. User answers.
3. Give one-line feedback: ✅ correct / ❌ + the right answer + one-sentence why.
4. Move to the next question.

Question formats (vary them):
- Fill in the blank: `"The model ___ accurate results." (produce/produces?)`
- Spot the error: `"What's wrong with this sentence: 'I want to improvement my English.'"`
- Rewrite: `"Make this more natural: 'How can I improvement?'"`

---

## Step 4: Vocabulary quiz (2–3 questions)

Pick 2–3 words/expressions from `wiki/vocabulary/` or `wiki/expressions/`, prioritizing `review_count: 0`.

For each:
1. Show the word only. Ask: `"Use [word] in a sentence."` **Stop. Wait.**
2. User answers.
3. Give brief feedback — confirm or suggest a better version.
4. Move on.

---

## Step 5: Wrap up

After all questions are done:
- Give a one-line overall impression ("Good focus today — subject-verb is getting cleaner.")
- Name the one thing to keep watching today.
- Ask: `"Want to keep going with a short roleplay, or are we done?"`

If they want more → do a 3–5 exchange mini roleplay based on recent topics in `wiki/topics/`.
If done → log the session in `wiki/log.md` as type `review` and increment `review_count` on drilled pages.

---

## Notes for Claude

- English throughout. Korean only if explaining a grammar rule for the first time.
- Keep feedback tight — one or two sentences max per answer.
- Total session: 5–10 minutes. Don't drag it out.
