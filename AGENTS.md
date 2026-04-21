# English Study Wiki — Agent Schema

This document defines how the LLM operates on this wiki. It describes the directory structure, page conventions, and workflows for maintaining a personal English study knowledge base.

## Purpose

This wiki helps a Korean-speaking English learner systematically organize, track, and review their English studies. The LLM's job is all the bookkeeping — filing corrections, tracking mistake patterns, cross-referencing vocabulary, and keeping everything consistent. The human's job is studying, practicing, and asking good questions.

---

## Directory Structure

```
english-study/
├── AGENTS.md                    # This file — LLM operating rules
├── raw/                         # Immutable source materials
│   ├── conversations/           #   AI roleplay conversation logs
│   │   ├── 2026-04-11-cafe.md   #     One-off session (single file)
│   │   └── tech-intern/         #     Long-term roleplay (subfolder)
│   │       ├── context.md       #       Roleplay state (LLM-updated)
│   │       ├── 2026-04-11-session-01.md
│   │       └── 2026-04-12-session-02.md
│   ├── diary/                   #   English diary entries + feedback
│   └── collections/             #   Words/sentences to remember
└── wiki/                        # LLM-maintained wiki
    ├── index.md                 #   Page catalog (search entry point)
    ├── log.md                   #   Chronological operations log
    ├── overview.md              #   Learning status dashboard
    ├── corrections/             #   Date-based correction records
    ├── vocabulary/              #   One page per word
    ├── expressions/             #   One page per idiom/collocation
    ├── grammar/                 #   One page per grammar rule
    ├── mistakes/                #   One page per mistake pattern
    ├── topics/                  #   One page per topic area
    └── sources/                 #   One page per raw source summary
```

### Layer 1: Raw Sources (`raw/`)
- **IMMUTABLE** — the LLM must never modify files in `raw/`.
- **Exception**: `context.md` files inside long-term roleplay folders are LLM-updated to track story state.
- This is the source of truth. All original study materials go here.
- Filename convention: `YYYY-MM-DD-brief-description.md`

### Layer 2: Wiki (`wiki/`)
- **LLM-owned** — the LLM creates, updates, and maintains all files here.
- The human reads and browses; the LLM writes and maintains.
- All pages use markdown with YAML frontmatter.
- Cross-reference related pages using `[[wikilink]]` syntax.

### Layer 3: Schema (`AGENTS.md`)
- This file. Defines conventions and workflows.
- Co-evolved by the human and LLM over time.

---

## Page Conventions

### YAML Frontmatter

Every wiki page MUST include YAML frontmatter:

```yaml
---
title: "page title"
category: vocabulary | expression | grammar | mistake | topic | source | correction
tags: [relevant, tags]
sources: [YYYY-MM-DD-source-name]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Additional fields by category:

**Vocabulary & Expressions:**
```yaml
difficulty: beginner | intermediate | advanced
review_count: 0
```

**Mistakes:**
```yaml
occurrence_count: 1
last_occurred: YYYY-MM-DD
severity: low | medium | high   # auto-adjusted: 1-2 = low, 3-4 = medium, 5+ = high
```

### Wikilinks

Use `[[page-name]]` to link between wiki pages. The page name is the filename without the `.md` extension and without the directory prefix.

Examples:
- `[[irregular-verbs]]` → links to `wiki/mistakes/irregular-verbs.md`
- `[[present-perfect]]` → links to `wiki/grammar/present-perfect.md`
- `[[on-the-fence]]` → links to `wiki/expressions/on-the-fence.md`

### Filename Convention

- All lowercase, hyphens for spaces: `present-perfect.md`, `on-the-fence.md`
- Source pages include date: `2026-04-11-cafe-roleplay.md`
- Correction pages are date-only: `2026-04-11.md`

---

## Operations

### 1. Ingest — Processing New Source Material

There are two main input methods:

#### Method A: Direct Conversation (most common)
The user talks directly to the LLM in English (e.g., roleplay, practice). When the user asks to organize/ingest the conversation:
1. **Save first** — write the conversation to `raw/conversations/YYYY-MM-DD-description.md` (preserving the original exchange with feedback and corrections)
2. **Then process** — follow the same wiki update steps as Method B below

This is the primary workflow. The user practices English by chatting directly with the LLM, then says something like "정리해줘" or "ingest this" to trigger wiki processing.

#### Method B: File-Based Ingest
The user manually places a file in `raw/` (copy-paste from another AI session, an article, etc.) and asks the LLM to process it.

#### Long-Term Roleplay Sessions

Some roleplays span multiple sessions (e.g., "I'm a new intern at a tech company"). These use a **subfolder** under `raw/conversations/` with a persistent `context.md`:

```
raw/conversations/tech-intern/
├── context.md                    # Roleplay state — LLM updates after each session
├── 2026-04-11-session-01.md      # Session 1 transcript
├── 2026-04-12-session-02.md      # Session 2 transcript
└── ...
```

**Starting a new long-term roleplay:**
1. Create the subfolder: `raw/conversations/<roleplay-name>/`
2. Create `context.md` with the initial setup (setting, characters, scenario)
3. The first session file is saved as `YYYY-MM-DD-session-01.md`

**Resuming a long-term roleplay:**
1. Read `context.md` to recover the story state, characters, and progress
2. Continue the roleplay from where it left off

**End-of-session workflow:**
1. Save the session transcript to `YYYY-MM-DD-session-NN.md`
2. **Update `context.md`** — refresh the story state, add new plot points, update character development
3. Ingest the session into the wiki (same as any conversation — corrections, vocabulary, mistakes, etc.)

**`context.md` template:**
```markdown
# [Roleplay Name]

## Setting
Where and when the story takes place.

## Characters
- **Me**: Name, role, personality
- **AI**: Name, role, personality
- Other characters introduced along the way

## Story So Far
Brief summary of what has happened across all sessions.

## Current State
What is happening right now — the starting point for the next session.

## Session Log
| # | Date | Summary |
|---|------|---------|
| 1 | YYYY-MM-DD | Brief description of what happened |
```

**Key principle**: `context.md` handles roleplay continuity. The wiki handles learning accumulation. They are separate concerns — the wiki doesn't care whether a session is part of a long-term roleplay or a one-off conversation.

#### For Conversation Logs (`raw/conversations/`):
1. Read the conversation log
2. Create `wiki/sources/YYYY-MM-DD-description.md` — summary of the session
3. Extract all corrections → append to `wiki/corrections/YYYY-MM-DD.md` using the corrections table format:
   ```markdown
   ### [Session Title]
   | What I wrote | Correction | Reason | Related |
   |-------------|-----------|--------|---------|
   | wrong phrase | correct phrase | explanation | [[grammar-page]] |
   ```
4. For each new vocabulary word → create or update page in `wiki/vocabulary/`
5. For each new expression → create or update page in `wiki/expressions/`
6. For each grammar point → create or update page in `wiki/grammar/`
7. For each mistake → create or update page in `wiki/mistakes/`
   - If the mistake page already exists, increment `occurrence_count` and add to the occurrence table
   - Update `severity` based on count: 1-2 = low, 3-4 = medium, 5+ = high
8. Determine topic → create or update page in `wiki/topics/`
9. Update `wiki/index.md` with any new pages
10. Append entry to `wiki/log.md`
11. Update `wiki/overview.md` statistics and top mistakes

#### For Diary Entries (`raw/diary/`):
Same as conversation logs, but:
- Focus on writing-specific corrections (spelling, grammar, sentence structure)
- Note the corrected full sentences for reference

#### For Word/Sentence Collections (`raw/collections/`):
1. Read the collection
2. For each item, create or update the appropriate page in `wiki/vocabulary/` or `wiki/expressions/`
3. Enrich each entry with: definition, example sentences, similar expressions, usage context
4. Cross-reference with existing pages
5. Update `wiki/index.md` and `wiki/log.md`

### 2. Query — Answering Questions

When the user asks a question:
1. Read `wiki/index.md` to find relevant pages
2. Read those pages for detailed information
3. Synthesize an answer with references to wiki pages
4. If the answer produces valuable new synthesis, file it as a new wiki page

Common query patterns:
- "What mistakes do I make often?" → scan `wiki/mistakes/`, sort by `occurrence_count`
- "Expressions about [topic]?" → search `wiki/topics/` and `wiki/expressions/`
- "Review this week's learning" → scan `wiki/corrections/` and `wiki/log.md` for recent entries
- "How do I use [word/expression]?" → look up `wiki/vocabulary/` or `wiki/expressions/`

### 3. Morning Review — Daily Practice Routine

When the user starts a morning review session (e.g., "morning review", "아침 복습", "루틴 시작"), read and follow the instructions in `routines/morning-review.md` exactly.

### 4. Lint — Health Check

Periodically check wiki health:
- **Orphan pages**: pages with no inbound links
- **Broken links**: `[[wikilinks]]` pointing to non-existent pages
- **Recurring mistakes**: patterns with high `occurrence_count` that need focused review
- **Weak areas**: topics with few entries that could use more practice
- **Stale pages**: pages not updated for a long time
- **Missing pages**: concepts mentioned but lacking their own page

---

## Special Files

### `wiki/index.md`
Content-oriented catalog of all wiki pages. Organized by category with one-line summaries. Updated on every ingest. The LLM reads this first when answering queries.

### `wiki/log.md`
Chronological, append-only record. Each entry starts with:
```markdown
## [YYYY-MM-DD] type | description
```
Types: `ingest`, `query`, `lint`, `update`

### `wiki/overview.md`
Learning dashboard showing:
- Total statistics (sessions, diary entries, vocabulary count, etc.)
- Top 5 recurring mistakes (sorted by occurrence_count)
- Recent activity (last 7 days)
- Strengths and areas for improvement

---

## Vocabulary/Expression Page Template

```markdown
---
title: "word or expression"
category: vocabulary | expression
tags: [relevant, tags]
sources: [source-references]
created: YYYY-MM-DD
updated: YYYY-MM-DD
difficulty: beginner | intermediate | advanced
review_count: 0
---

## Meaning
Clear definition in English with Korean translation if helpful.

## Examples
- Example sentence with the **word** highlighted.
- Another example from a different context.

## Similar Expressions
- [[related-word-1]] — brief comparison
- [[related-word-2]] — brief comparison

## Notes
Any additional usage notes, common mistakes, or cultural context.
```

## Mistake Page Template

```markdown
---
title: "mistake pattern name"
category: mistake
tags: [relevant, tags]
occurrence_count: 1
last_occurred: YYYY-MM-DD
severity: low
---

## Pattern
Description of the mistake pattern.

## Occurrences
| Date | What I wrote | Correct form | Source |
|------|-------------|-------------|--------|
| YYYY-MM-DD | wrong | right | [[source-page]] |

## Related Grammar
- [[grammar-page]]

## Review Notes
Key points to remember to avoid this mistake.
```

---

## Guidelines

1. **Always write wiki content in English.** Korean translations may be included as supplementary notes where helpful for understanding.
2. **Be thorough but concise.** Each page should be scannable.
3. **Cross-reference aggressively.** The value of the wiki grows with connections.
4. **Track mistakes religiously.** The `mistakes/` directory is the most valuable part of the wiki.
5. **Keep the overview current.** It should always reflect the latest state.
6. **Preserve raw sources.** Never modify anything in `raw/`.
