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

Curate only when knowledge management or durable learning is selected. Retain
evidence-backed information that will help a future task or decision. Exclude
session diaries, transcripts, redundant explanations, facts obvious from the
code, and unsupported claims.

### Choose the change

Read the relevant existing pages before deciding, checking for equivalent meaning
even when wording or titles differ. Choose the smallest useful change:

- **Skip** when the information is temporary, unsupported, already covered, or
  offers no durable value. No change is a valid result.
- **Update** when an existing page is the right home for a correction, new
  evidence, or a useful qualification.
- **Merge** when overlapping material would be easier to find and maintain in
  one place, while preserving unique facts, exceptions, and evidence.
- **Split** when a page mixes distinct topics and separating them improves
  retrieval without losing their relationships.
- **Create** when useful durable information has no suitable existing home.

### Resolve contradictions

Check the evidence and applicability of conflicting claims, including versions
and conditions. Preserve sources and context, and explain why a correction or
supersession is justified. If the evidence does not resolve the disagreement,
preserve and report it explicitly; do not silently choose the latest claim.

### Keep organization within scope

Reorganize only within the approved scope. Maintain affected navigation and
references, preserving unique exceptions, evidence, and relationships when
moving or consolidating content. Propose broader changes separately when they
would exceed that scope.

- Use the target's existing categories for internal discoveries, decisions,
  external-source compilations, and raw source material. Preserve provenance and
  category separation, including protection/locking rules during reorganization.
- Indexes hold navigation, not detailed knowledge, where the target uses them.
- Update the configured action log for knowledge changes and run the configured
  knowledge linter when available.
