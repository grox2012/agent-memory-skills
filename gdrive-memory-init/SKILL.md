---
name: gdrive-memory-init
description: Initialize a Google Drive-backed agent memory filesystem inspired by OpenViking. Use when the user wants to set up memory storage, create the manifest/index documents, establish mem:// URI rules, or add a system prompt that tells an agent how to use the memory skills.
---

# Google Drive Memory Init

Use this skill to initialize or repair a Google Drive-backed memory filesystem. This is an OpenViking-inspired simulation: Drive stores the content and index artifacts; the agent maintains L0/L1/L2 semantics.

Open `references/memoryfs-schema.md` when you need the exact schema, URI layout, or bootstrap templates.

## Model

- URI scheme: `mem://resources/...`, `mem://user/memories/...`, `mem://agent/memories/...`, `mem://agent/skills/...`, `mem://session/...`.
- L0: one short abstract, usually under 100 tokens, stored in the index row.
- L1: overview/navigation summary, usually under 2k tokens, stored in the index row and the content doc.
- L2: full content, stored in a Google Doc or other Drive file.
- Index: one Google Sheet named `Agent MemoryFS Index`.
- Manifest: one Google Doc named `Agent MemoryFS Manifest`.

## Initialization Workflow

1. Search Google Drive for `Agent MemoryFS Manifest` and `Agent MemoryFS Index`.
2. If both exist, read them and report the detected root. Do not duplicate them unless the user asks for a fresh memory system.
3. If missing, create:
   - Google Doc: `Agent MemoryFS Manifest`
   - Google Sheet: `Agent MemoryFS Index`
4. Populate the index sheet headers from `references/memoryfs-schema.md`.
5. Write the manifest content from the template in `references/memoryfs-schema.md`.
6. Create starter index rows for root directories:
   - `mem://resources/`
   - `mem://user/memories/`
   - `mem://agent/memories/`
   - `mem://agent/skills/`
   - `mem://session/`
7. Return the created Drive file titles, links/IDs when available, and the system prompt block.

## Drive Tool Guidance

- Prefer native Google Docs/Sheets through the Google Drive connector.
- Search before creating. Title collisions are common in Drive.
- If true Drive folders cannot be created by the available connector, use document titles and the index as the logical filesystem source of truth.
- Never store secrets, passwords, private keys, or sensitive personal data unless the user explicitly confirms.

## System Prompt Injection

When asked to provide the memory system prompt, include this concise block:

```text
You have access to a Google Drive-backed MemoryFS that simulates OpenViking-style agent memory. Treat the index sheet as the filesystem index and Drive docs/files as content storage. Use L0 abstracts for quick filtering, L1 overviews for navigation, and load L2 full content only when needed. Valid roots are mem://resources/, mem://user/memories/, mem://agent/memories/, mem://agent/skills/, and mem://session/. Before answering memory-sensitive tasks, retrieve relevant memories with the gdrive-memory-retrieve workflow. When the user teaches a durable preference, fact, decision, project detail, reusable case, tool pattern, or skill lesson, write it with the gdrive-memory-write workflow. Keep facts source-backed, update mergeable memories instead of duplicating them, and record all writes in the audit log columns.
```

## Completion Criteria

- Manifest and index exist in Google Drive, or the user is told exactly what is missing.
- The index has the required columns.
- Root URI rows exist.
- The user receives the system prompt and the next steps for using write/retrieve skills.
