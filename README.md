# Conversation Archive

A skill for AI assistants to generate structured, meaningful conversation archives — not just transcripts, but living documents that capture what changed.

## The Problem

Most conversation archiving gives you two bad options:

- **Raw transcripts**: searchable but unreadable. Good luck finding that one insight from three months ago.
- **AI summaries**: clean but hollow. The hesitation, the breakthrough moment, the thing you said that surprised even yourself — all smoothed away into "user expressed growth."

## What This Skill Does

This skill sits between the two. It gives your AI assistant a protocol for archiving conversations that is:

- **Structured enough** to be useful months later (metadata, tags, key outputs, action items)
- **Textured enough** to preserve what actually happened (direct quotes, emotional tone, hesitation markers)
- **Safe by default** (append-only updates, confirmation gates before any destructive changes)

## Two Modes

### Quick Mode (no setup)

One command, one archive file. Works for any conversation — work sessions, brainstorms, debugging, planning.

### Deep Mode (for ongoing relationships)

For conversations that happen repeatedly with the same person — coaching, therapy, mentoring, journaling, any context where growth matters over time. Maintains three living documents:

| Document | Question It Answers |
|----------|-------------------|
| **Profile** | Who is this person *right now*? |
| **History** | What did we talk about each time? |
| **Changelog** | What were the turning points? |

Plus an optional **Content Harvesting** module for people who create content (podcasts, blogs, videos) — it surfaces moments from conversations that could become raw material.

## Core Design Principles

### 1. Append First, Rewrite Rarely
Never silently delete or replace existing content. New information gets added; old information stays unless the user explicitly approves a rewrite.

### 2. Preserve Texture
AI naturally wants to give you clean conclusions. But "I still can't believe this is happening" carries information that "user expressed surprise at change" does not. The skill preserves hesitation, direct quotes at emotional peaks, and tentative language — because those ARE the evidence that something real happened.

### 3. Confirmation Gates
Rewriting someone's "current state" in their profile is a high-stakes operation. The skill requires four conditions before any rewrite: old state preserved elsewhere, new state genuinely supersedes the old, user confirms, and texture markers carry forward.

### 4. Only What's Real
Archive what the person actually said and realized. Not what the AI inferred or projected.

## Quick Start

1. Add `conversation-archive.md` to your AI assistant's skill/instruction set
2. Have a conversation
3. Say "archive this" (or "wrap up", "save this conversation", etc.)

For Deep Mode, create `profile.md`, `history.md`, and `changelog.md` in your workspace. The skill auto-detects and switches modes.

## Customization

The skill is designed to be forked and adapted:

- Edit trigger phrases in the frontmatter
- Add/remove archive sections
- Extend the profile structure with your own sections
- Remove Content Harvesting if you don't need it
- Works in any language — follows the conversation by default

See the [full skill file](conversation-archive.md) for detailed customization points.

## Design Philosophy

This skill navigates three tensions:

1. **Structure vs. texture.** Enough structure to find things later. Enough texture to trust what you find.

2. **Continuity vs. snapshot.** The profile is always "now." The history is always "then." The changelog is "when things turned." They never collapse into each other.

3. **Efficiency vs. safety.** Append is slower but safe. Rewrite is faster but destructive. Default is always the safe path.

## Related projects

Other open-source tools by the same author:

- [critical-second-pass](https://github.com/FlyingFSR/critical-second-pass) — a
  Codex skill for an honest, harder second read of a plan, diff, or draft.
- [Friday](https://github.com/FlyingFSR/friday) — local-first, on-device voice
  input for macOS (whisper.cpp, no cloud).

## License

MIT
