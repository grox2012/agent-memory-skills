---
name: gdrive-memory-retrieve
description: Retrieve relevant context from a Google Drive-backed MemoryFS using OpenViking-style hierarchical search. Use when the user asks what the agent remembers, when a task may depend on stored preferences or project context, or when an answer should be grounded in prior memory/resources/skills.
---

# Google Drive Memory Retrieve

Use this skill to read from the Google Drive-backed MemoryFS. Retrieval should be deliberate: scan L0 abstracts first, use L1 overviews for navigation, then load L2 full content only for the entries that matter.

Open `references/retrieval-schema.md` for root mappings and scoring guidance.

## Retrieval Workflow

1. Locate `Agent MemoryFS Index` in Google Drive. If missing, use `gdrive-memory-init`.
2. Convert the task into 1-5 typed queries:
   - `memory`: user profile, preferences, entities, events, agent cases/patterns/tools/skills.
   - `resource`: docs, project material, rules, references.
   - `skill`: callable capabilities and skill definitions.
   - `session`: archived sessions and memory diffs.
3. Pick root directories:
   - memory: `mem://user/memories/`, `mem://agent/memories/`
   - resource: `mem://resources/`
   - skill: `mem://agent/skills/`
   - session: `mem://session/`
4. Search the index sheet first using URI prefix, tags, names, L0 abstracts, and L1 overviews.
5. Rank candidates by:
   - direct URI/path match
   - category fit
   - L0 relevance
   - L1 relevance
   - recency when the user asks for recent context
   - active status, excluding `deleted` and usually excluding `superseded`
6. Read L1 overviews for top candidates if available in the Drive doc or index.
7. Read L2 full content only when L1 indicates the detail is needed.
8. Return a compact result:
   - answer or retrieved context
   - cited memory URIs and Drive titles/links when available
   - confidence and gaps
   - whether more L2 reading would be useful

## Read Policy

- Do not dump all memory. Retrieve only what the task needs.
- If results conflict, present the conflict and cite both memories.
- Treat memories as evidence, not truth. Prefer newer or higher-confidence entries, but do not erase older context unless the user asks to update memory.
- When memory is missing, say so clearly and continue from available context.

## Response Shape

For direct recall:

```text
I found:
- {fact} — {uri}, source {source_refs}, confidence {confidence}

Gaps:
- {missing_or_uncertain_context}
```

For task grounding:

```text
Relevant memory:
- {short context}

How I will use it:
- {task-specific implication}

Sources:
- {uri} / {Drive title}
```
