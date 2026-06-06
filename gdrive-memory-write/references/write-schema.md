# Memory Write Schema

## Index Row Fields

Required fields for every write:

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

## Category Rules

| category | path | strategy |
| --- | --- | --- |
| profile | `mem://user/memories/profile.md` | merge |
| preferences | `mem://user/memories/preferences/{topic}.md` | merge or append |
| entities | `mem://user/memories/entities/{entity}.md` | merge or append |
| events | `mem://user/memories/events/{date}-{slug}.md` | append-only |
| cases | `mem://agent/memories/cases/{date}-{slug}.md` | append-only |
| patterns | `mem://agent/memories/patterns/{topic}.md` | merge |
| tools | `mem://agent/memories/tools/{tool}.md` | merge |
| skills | `mem://agent/memories/skills/{skill}.md` | merge |
| resources | `mem://resources/{project_or_topic}/{slug}.md` | create or replace with user confirmation |

## L2 Content Template

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
{what_this_contains_when_to_read_l2}

## L2 Detail
{full_content}

## Relations
- {related_uri}: {reason}

## Audit
- {timestamp}: {create|merge|append|supersede}; reason: {reason}
```

## Dedup Heuristic

Before writing, look for:

- Exact URI match.
- Same category and slug.
- Same entity/project/person name.
- Overlapping tags.
- Similar L0 or L1 text in the index.

If there is a likely match, prefer merge unless the category is append-only.
