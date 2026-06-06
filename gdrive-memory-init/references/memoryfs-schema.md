# Google Drive MemoryFS Schema

This schema simulates the OpenViking concepts of context types, L0/L1/L2 layers, URI-based access, and dual storage/index separation using Google Drive.

## Logical Roots

```text
mem://
├── resources/             # Static knowledge, docs, rules, references
├── user/
│   └── memories/          # User profile, preferences, entities, events
├── agent/
│   ├── memories/          # Agent cases, patterns, tool lessons, skill lessons
│   └── skills/            # Skill definitions or links
└── session/               # Session archives and memory diffs
```

## Memory Categories

| category | root | update strategy |
| --- | --- | --- |
| profile | `mem://user/memories/profile.md` | merge into one file |
| preferences | `mem://user/memories/preferences/` | append or merge by topic |
| entities | `mem://user/memories/entities/` | append or merge by entity |
| events | `mem://user/memories/events/` | append-only |
| cases | `mem://agent/memories/cases/` | append-only |
| patterns | `mem://agent/memories/patterns/` | mergeable |
| tools | `mem://agent/memories/tools/` | mergeable |
| skills | `mem://agent/memories/skills/` | mergeable |

## Index Sheet

Name: `Agent MemoryFS Index`

Create these headers in row 1:

```text
uri
parent_uri
name
context_type
category
is_dir
is_leaf
l0_abstract
l1_overview
drive_file_id
drive_url
mime_type
tags
relations
source_refs
confidence
created_at
updated_at
active_count
status
audit_note
```

Allowed `context_type`: `resource`, `memory`, `skill`, `session`.

Allowed `status`: `active`, `superseded`, `deleted`, `draft`.

Use ISO-8601 UTC timestamps.

## Content Doc Template

Use this structure for each L2 Google Doc unless a source file already has its own format:

```markdown
# {name}

URI: {uri}
Context type: {context_type}
Category: {category}
Created: {created_at}
Updated: {updated_at}
Source refs: {source_refs}
Confidence: {confidence}

## L0 Abstract
{under_100_tokens}

## L1 Overview
{navigation_summary_and_scope}

## L2 Detail
{full_memory_or_resource_content}

## Relations
- {related_uri}: {reason}

## Audit
- {timestamp}: created/updated by agent; reason: {reason}
```

## Manifest Template

```markdown
# Agent MemoryFS Manifest

This Google Drive account stores an OpenViking-inspired memory filesystem.

## Files
- Index: Agent MemoryFS Index
- Content: Google Docs or Drive files referenced by index rows

## Roots
- mem://resources/
- mem://user/memories/
- mem://agent/memories/
- mem://agent/skills/
- mem://session/

## Read Policy
Use L0 for quick filtering, L1 for navigation, and L2 only when detail is necessary.

## Write Policy
Search for related memories before writing. Merge profile, preference, entity, pattern, tool, and skill memories when possible. Append events and cases. Keep source references and audit notes.
```
