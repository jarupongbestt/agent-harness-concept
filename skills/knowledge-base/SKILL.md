---
name: knowledge-base
description: Navigate, update, and lint the durable project knowledge tree while separating internal, derived, and raw source material.
---

# Knowledge Base

Use the durable knowledge tree as project memory, not as run transcript storage.

## Read

- Always read `knowledge/main.md` at run startup.
- Follow its links to `knowledge/domain/index.md` and then the matching topic.
- Read `knowledge/log.md` only for historical activity, contradictions, recurring
  failures, audit evidence, or lint context.
- Read files under `sources/` only when the specific external evidence is needed.

## Write

- `self/` holds internal discoveries, decisions, constraints, and gotchas.
- `derived/` holds faithful external-source compilations and requires provenance.
- `sources/` holds raw external material and is protected from normal edits.
- Indexes hold navigation, not detailed knowledge.
- Update the action log for knowledge changes, then run the knowledge linter.

Never silently overwrite contradictions or store the complete conversation.
