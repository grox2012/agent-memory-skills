# Agent Memory Skills

OpenViking-inspired Google Drive-backed agent memory skills.

## Skills

- `gdrive-memory-init`: initialize a Google Drive-backed MemoryFS manifest and index.
- `gdrive-memory-write`: write durable memories, resources, and agent lessons.
- `gdrive-memory-retrieve`: retrieve relevant memory through L0/L1/L2 hierarchical search.

## MemoryFS Shape

The skills simulate an OpenViking-style memory filesystem using Google Drive as storage:

- `mem://resources/`
- `mem://user/memories/`
- `mem://agent/memories/`
- `mem://agent/skills/`
- `mem://session/`

The index lives in a Google Sheet named `Agent MemoryFS Index`; L2 content lives in Google Docs or Drive files.
