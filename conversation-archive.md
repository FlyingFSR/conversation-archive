---
name: conversation-archive
description: >
  Generates structured markdown archives of conversations with optional living-profile
  tracking across sessions. Triggered by: "archive", "wrap up", "summarize this session",
  "save this conversation", "let's call it a day", or any intent to close and preserve
  a conversation. Supports Quick mode (single archive) and Deep mode (archive + evolving
  profile + changelog).
---

# Conversation Archive

A skill for AI assistants to generate structured, meaningful conversation archives —
not just transcripts, but living documents that capture what changed.

---

## Why This Exists

Most conversation archiving is either "dump the transcript" or "generate a summary."
Both lose something important:

- Raw transcripts are searchable but unreadable
- AI summaries are clean but strip texture — the hesitation, the breakthrough moment,
  the thing someone said that surprised even themselves

This skill sits between the two: structured enough to be useful months later,
textured enough to preserve what actually happened.

---

## Two Modes

| Mode | Output | Use When |
|------|--------|----------|
| **Quick** | 1 archive file | One-off conversations, work sessions, meetings, brainstorms |
| **Deep** | 1 archive file + profile/history/changelog updates | Ongoing relationships — coaching, therapy, mentoring, journaling, any conversation series where growth matters |

**Mode detection**: If the workspace contains `profile.md` and `history.md`,
use Deep mode. Otherwise, default to Quick mode.

---

## Quick Mode

Generate a single archive file:

```markdown
# Archive: [Topic]

| Field | Value |
|-------|-------|
| Date | YYYY-MM-DD |
| Tags | [tag1, tag2] |
| Type | [reflection / planning / debugging / brainstorm / decision-making / ...] |

## Summary

> 2-3 sentences. What happened and what came out of it.

## Key Outputs

- Output 1
- Output 2

## Action Items

- [ ] Todo 1

## Insights & Learnings

[Use whichever subsections apply — don't include empty ones]

### Concepts & Terminology
### Tools & Methods
### Cognitive Updates
### Pitfalls & Warnings

## Notes

[If any]

## Conversation Record

[Full conversation with standardized role markers]
```

No setup needed. Just ask your AI to "archive this conversation" with this skill loaded.

---

## Deep Mode

Everything in Quick mode, plus updates to three living documents:

| Document | Question It Answers | Update Style |
|----------|-------------------|--------------|
| `profile.md` | Who is this person *right now*? | Current state = rewrite; patterns/strategies = append |
| `history.md` | What did we talk about each time? | Append new entry per session |
| `changelog.md` | What were the turning points? | Append when warranted |

---

## Core Principles

These four principles apply to both modes but matter most in Deep mode, where
the skill maintains a persistent model of a person over time.

### 1. Append First, Rewrite Rarely

Default to appending new information. Never silently delete, replace, or compress
existing content.

Allowed exceptions:
- Header metadata updates (dates, session counts)
- High-confidence mechanical fixes (typos, formatting, structural drift)
- Semantic changes explicitly approved by the user

"Structural drift" here means only non-semantic formatting issues: wrong heading
levels, duplicate headings, list marker or indentation drift. It does NOT include
reorganizing paragraphs, merging entries, changing categories, or rewriting logic.

### 2. Preserve Texture (Resist "Gentle Rationalization")

AI naturally smooths rough edges into clean conclusions. But real change often
arrives with hesitation, disbelief, and trembling — that texture IS the evidence
that change is happening. Stripping it produces a polished lie.

**Rule A — Keep the hesitation.**

Words like "maybe," "I think," "for the first time," "I still can't believe"
must survive into the archive. Don't upgrade tentative insights into firm conclusions.

Match the person's actual psychological state:

| What they showed | What you can write |
|------------------|--------------------|
| Certain, confirmed repeatedly | "established," "committed to" |
| Half-believing, exploring | "beginning to sense," "leaning toward" |
| Confused, just trying to say it | Do NOT frame as a conclusion |

**Rule B — Direct-quote emotional peaks.**

When someone says something at a moment of high emotion — tears, surprise at
themselves, a long silence — preserve at least one direct quote in the archive.
Don't paraphrase everything into third-person summary.

> "How did this even happen to me?"

is not the same as

> "User expressed surprise at their own change."

The person's own words carry weight and rhythm that no summary reproduces.

### 3. Confirmation Gates for Destructive Updates

Rewriting someone's "current state" in their profile is high-stakes — it replaces
how the system understands them *right now*.

Before any current-state rewrite, ALL four conditions must be met:

1. The old state is preserved somewhere retrievable (history entry or changelog)
2. The new state genuinely supersedes (not merely extends) the old
3. The user explicitly confirms the rewrite
4. Texture markers from the old version carry forward where still relevant

If any condition isn't met, don't rewrite — record the new information in the
history log instead.

### 4. Extract What's Real, Not What's Inferred

Only archive information that explicitly appeared in the conversation.
Record what the user said and realized — not what the AI speculated or projected.

---

## Profile Management (Deep Mode)

The profile answers: "Who is this person right now?"

It is a current-state snapshot, not a chronological record. Evolution is tracked
by the history log (every session) and changelog (turning points).

### Recommended Structure

```markdown
# Profile

> Last updated: YYYY-MM-DD
> Total sessions: N

## Core Themes

### [Theme Name]
- **Current state**: [Living paragraph — rewrite when understanding shifts]
- **Key trajectory**: A(MM-DD) → B(MM-DD) → C(MM-DD)
- **Connections**: [Links to other themes]

## Patterns & Traits
- **[Pattern]**: [Description] (MM-DD)

## Effective Strategies
- **[Strategy]**: [What works and why] (MM-DD)

## Cognitive Progress
- **From X to Y** (MM-DD) — see Changelog [entry]

## Conversation Preferences
- [How this person likes to communicate]
```

### Update Rules by Section

| Section | How | When |
|---------|-----|------|
| Current state | Rewrite (with confirmation) | Genuine cognitive shift |
| Trajectory | Append node | Direction change |
| Patterns | Append | New pattern discovered |
| Strategies | Append | New strategy confirmed |
| Cognitive progress | Add index line → changelog | Each shift |
| Preferences | Append | New preference found |

### When Does Current State Get Rewritten?

| Situation | Action |
|-----------|--------|
| **Genuine shift**: new understanding *replaces* the old | Rewrite current state |
| **Direction change**: stance or strategy visibly pivots | Rewrite + add trajectory node |
| **Validation**: conversation confirms existing understanding | Don't rewrite. Log in history |
| **Extension**: same insight applied in new context | Don't rewrite. Maybe add to patterns |
| **High emotion, no new insight** | Don't rewrite. Capture emotion in history |

### Creating New Core Themes

All three must be true:
1. Doesn't fit inside any existing theme
2. Has its own independent core concern
3. Has potential to develop across future sessions

If any condition is false → file under an existing theme.

---

## History Log (Deep Mode)

One entry per session. Append-only. Never rewrite or remove old entries.

```markdown
## YYYY-MM-DD ｜ [Topic Keywords]

**Core content**: [What was actually discussed]

**Key moments**: [Breakthroughs, surprises, important exchanges]

**Emotional tone**: [The texture of how it felt]

**Connections**: [Links to themes, past sessions, changelog entries]
```

---

## Changelog (Deep Mode)

A dual-track record of what mattered.

### Part 1 — Decisions & Cognitive Shifts

Triggered by: decisions made, understanding that shifted, "I figured it out,"
"I've decided," "I see it differently now."

```markdown
## YYYY-MM ｜ [Decision / Shift Title]
**Decision**: [...] / **Shift**: [...] / **Context**: [...]
```

### Part 2 — Life Events & Updates

Triggered by: concrete events, recent news, small moments worth remembering.

```markdown
- **YYYY-MM**: [One-line description]
```

### Proactive Changelog Prompt

Before finalizing a Deep mode archive, proactively ask:

1. **Part 1**: If anything in the conversation looks like a decision or shift,
   surface it and confirm
2. **Part 2**: Ask directly — "Anything from your life recently worth noting?
   Even small things, especially the kind you'd forget if you don't write them
   down now."
3. If the user says no, don't force it

---

## Content Harvesting (Optional Module)

For users who create content (podcasts, blogs, videos, newsletters), conversations
are a rich source of raw material. This module surfaces reusable moments.

### What Qualifies

- The user explained a concept with vivid imagery or metaphor
- A passage has natural structure: scene → detail → resonance point
- An original framework, naming, or mental model emerged
- Something they said could go directly into content with minimal editing

### What Doesn't Qualify

- AI analysis or summaries (that's the AI's output, not the user's voice)
- Pure emotional venting (unless the expression itself is powerful)
- Insights already fully captured in profile or changelog

### Harvesting Flow

1. Identify 1-3 candidates from the conversation
2. For each: quote the user's words + one line on why it has content value
3. Ask: "Want to claim any of these for your material library?"
4. Claimed items get appended to `material-library.md`
5. If nothing qualifies, skip entirely — never force it

### Material Library Format (append-only)

```markdown
## YYYY-MM-DD ｜ [Material Name]

**In their words**:
> [Original quote, preserving spoken quality — don't clean it into formal prose]

**Why this works**: [One line]

**Source**: Archive YYYY-MM-DD_topic.md
```

---

## Update Protocol (Deep Mode)

### Diff Format

When generating updates for living documents, use explicit operation blocks
so every change is auditable:

```
## Header Update:
> Last updated: YYYY-MM-DD
> Total sessions: [N]

## Operations:

### Op 1: Append new entry
- Location: [Section name]
- Content: [What to add]

### Op 2: Rewrite current state [requires user confirmation]
- Location: [Theme name]
- Type: Current state rewrite
- Old state preserved in: History [date] / Changelog [entry]
- Rewrite to:
  [Full replacement paragraph, with texture preserved]
- Reason: [Why old state no longer reflects reality]

### Op 3: Metadata update
- Location: [Header field]
- Change: [Old] → [New]
```

### Self-Check Table

Every update set must include a comparison table to prevent drift:

| Location | Original (first 30 chars) | Operation | New content (first 30 chars) |
|----------|--------------------------|-----------|------------------------------|
| [Theme] — current state | "Has been exploring..." | Rewrite | "Now sees this as..." |
| [Theme] — trajectory | "...→ C (03-07)" | Append | "→ D (03-18)" |
| Header | "> Last updated: ..." | Metadata | "2025-03-18" |

### Post-Update Verification

1. List all backup file paths created
2. Itemize each change with location references
3. Word-count comparison for each modified file: before → after
4. **[WARN]** If any file got shorter, explain exactly why. If you can't explain
   it, stop and alert the user
5. If conversation record was generated from a transcript source, verify turn
   count matches

---

## Getting Started

### Quick Mode
No setup. Load this skill, have a conversation, say "archive this."

### Deep Mode

Create these files in your workspace:

1. **`profile.md`** — Start with a basic `## Core Themes` section
2. **`history.md`** — Empty file; grows one entry per session
3. **`changelog.md`** — Start with two headings: `## Part 1: Decisions & Shifts`
   and `## Part 2: Life Events`
4. **`material-library.md`** (optional) — Only if you create content

Deep mode activates automatically when `profile.md` and `history.md` are detected.

### Customization Points

| What | How |
|------|-----|
| Trigger phrases | Edit the frontmatter `description` |
| Archive sections | Add or remove from the Quick mode template |
| Profile structure | Add custom sections — the skill preserves whatever exists |
| Content harvesting | Delete the module entirely if you don't need it |
| Language | The skill follows conversation language by default |
| File paths | Rename the four files to whatever fits your system |

---

## Design Philosophy

This skill is built on a belief: **conversations are primary sources, not disposable
inputs.**

A good archive system doesn't just store — it helps you see patterns you couldn't
see in real time. But it earns that by being honest about what actually happened,
not what the AI thinks *should* have happened.

Three tensions this skill navigates:

1. **Structure vs. texture.** Enough structure to be findable six months later.
   Enough texture to still feel true.

2. **Continuity vs. snapshot.** The profile is always "now." The history is always
   "then." The changelog is "when things turned." They never collapse into each other.

3. **Efficiency vs. safety.** Append is slower but safe. Rewrite is faster but
   destroys. The default is always the safe path; the fast path requires explicit
   permission.

Every rule in this skill traces back to one of these three tensions.

---

## License

MIT — use it, fork it, adapt it.
