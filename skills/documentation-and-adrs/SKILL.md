---
name: documentation-and-adrs
description: Record durable decisions, constraints, procedures, and recurring failures without copying task transcripts.
---

# Documentation and ADRs

Capture knowledge that will change future decisions.

- Put internal discoveries, decisions, constraints, and gotchas in `self/`.
- Put faithful compilations of external sources in `derived/`, with provenance.
- Put raw external input in `sources/`; do not mix interpretation into it.
- Keep indexes navigational and concise.
- Record contradictions instead of silently overwriting one side.
- Update `log.md` only for knowledge actions, audit, or lint-relevant history.
- Run the knowledge linter after knowledge updates.

Do not store the complete conversation or a routine task diary.
