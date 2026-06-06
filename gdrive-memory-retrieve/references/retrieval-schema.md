# Memory Retrieval Schema

## Typed Query Roots

| query type | roots |
| --- | --- |
| memory | `mem://user/memories/`, `mem://agent/memories/` |
| resource | `mem://resources/` |
| skill | `mem://agent/skills/` |
| session | `mem://session/` |

## Candidate Filters

Use these filters before reading full content:

- `status = active`
- URI prefix starts with the selected root
- category matches the typed query when known
- tags/name/L0/L1 contain query terms or close synonyms
- source refs match the requested source when applicable

## Ranking Rubric

Score qualitatively:

| signal | weight |
| --- | --- |
| Exact URI or entity match | very high |
| Category/root match | high |
| L0 abstract relevance | high |
| L1 overview relevance | medium-high |
| Source/date match | medium |
| Active count and recency | low-medium |

When top candidates are directories, recursively inspect child rows whose `parent_uri` is the directory URI. Stop when the best results stop changing or enough evidence answers the task.

## L0/L1/L2 Use

- L0: quick filtering, do not over-trust.
- L1: usually enough for planning and deciding which L2 docs to open.
- L2: read for exact facts, quotes, decisions, instructions, or task-critical details.

## Output Evidence

Always preserve enough provenance for the user to identify the source:

```text
{uri} | {Drive title or file id} | {source_refs} | confidence={high|medium|low}
```
