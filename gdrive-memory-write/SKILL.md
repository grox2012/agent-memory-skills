---
name: gdrive-memory-write
description: Write durable memories, resources, skill lessons, and session archives into a Google Drive-backed MemoryFS. Use when the user wants the agent to remember something, add knowledge to memory, update preferences, record decisions, or store reusable agent experience.
---

# Google Drive Memory Write

Use this skill to add or update MemoryFS entries stored in Google Drive. It mirrors OpenViking's separation of content storage and index storage: Drive docs/files hold L2 content; the index sheet holds URI, L0, L1, metadata, and relations.

Open `references/write-schema.md` when you need category rules or the row format.

## Write Workflow

1. Confirm the content is durable enough to remember. Do not write transient chat, secrets, credentials, or unsupported personal data.
2. Locate `Agent MemoryFS Index` and `Agent MemoryFS Manifest` in Google Drive. If missing, use `gdrive-memory-init` first.
3. Classify the entry:
   - `resource`: external knowledge, docs, rules, project material.
   - `memory`: user or agent learned knowledge.
   - `skill`: callable capability definition.
   - `session`: archived session summary and memory diff.
4. Choose the target URI using the schema:
   - User profile: `mem://user/memories/profile.md`
   - Preferences: `mem://user/memories/preferences/{topic}.md`
   - Entities: `mem://user/memories/entities/{entity}.md`
   - Events: `mem://user/memories/events/{yyyy-mm-dd}-{slug}.md`
   - Agent cases: `mem://agent/memories/cases/{yyyy-mm-dd}-{slug}.md`
   - Agent patterns/tools/skills: topic-based mergeable files.
5. Search the index for same URI, same entity/topic, overlapping tags, and semantically similar L0/L1 text.
6. Decide write mode:
   - `create`: no sufficiently similar entry exists.
   - `merge`: update an existing mergeable memory with new evidence.
   - `append`: add append-only event/case or a dated audit item.
   - `supersede`: mark old row `superseded` only when the new entry replaces it.
7. Generate:
   - L0 abstract: under 100 tokens.
   - L1 overview: navigation summary with what is inside and when to read L2.
   - L2 detail: full memory with source refs, confidence, and audit.
8. Create or update the Google Doc for L2.
9. Create or update the corresponding index sheet row.
10. Report the URI, category, write mode, Drive file title/link, and any merge/supersede action.

## Quality Rules

- Keep verified facts separate from inference.
- Include `source_refs` whenever possible: email subject/date, calendar event title/date, Slack channel/thread, document title, or user-provided note.
- Use conservative confidence: `high`, `medium`, or `low`.
- Mergeable memories should become cleaner and more complete, not a pile of repeated bullets.
- Append-only memories should never be silently rewritten; add a new dated record.

## Memory Extraction Prompt

When extracting memory from a session, ask yourself:

```text
What durable user profile facts, preferences, entities, decisions, events, agent cases, reusable patterns, tool lessons, or skill lessons should survive future sessions? Which are verified by source evidence? Which existing memories should be merged, superseded, or left untouched?
```

Only write the items that pass this filter.
