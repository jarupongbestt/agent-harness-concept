---
name: knowledge-base
description: Navigate, update, and lint the durable project knowledge tree while separating internal, derived, and raw source material.
---

# Knowledge Base

Use the target project's configured durable knowledge system as project memory,
not as run transcript storage. Resolve its paths, categories, and write policy
from the approved adapter map or project instructions before acting. This
repository's `knowledge/domain/` tree is an example only.

## Read

- Read the configured knowledge startup entry at run startup when the selected
  capability requires it.
- Follow that entry's navigation to the relevant index and topic; do not presume
  any particular folder or filename.
- Read the configured action log only for historical activity, contradictions,
  recurring failures, audit evidence, or lint context, when such a log exists.
- Read files under `sources/` only when the specific external evidence is needed.

## Write

- Use the target's existing categories for internal discoveries, decisions,
  external-source compilations, and raw source material. Preserve provenance and
  protection/locking rules.
- Indexes hold navigation, not detailed knowledge, where the target uses them.
- Update the configured action log for knowledge changes and run the configured
  knowledge linter when available.

Never silently overwrite contradictions or store the complete conversation.
