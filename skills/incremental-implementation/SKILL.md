---
name: incremental-implementation
description: Implement one approved task slice with the smallest coherent, reversible change.
---

# Incremental Implementation

Implement only the approved slice.

- Read the relevant instructions, knowledge pages, and existing code first.
- Preserve behavior outside the acceptance criteria.
- Avoid unrelated cleanup, speculative abstractions, and silent scope expansion.
- Keep production and test changes aligned with the approved plan; never weaken
  a check to make it pass.
- Run the narrowest useful verification and report changed files, commands,
  results, and unresolved concerns.
- Do not commit, push, merge, or create branches unless separately authorized.

When a check fails, use `root-cause` and retry the same slice if its scope and
assumptions remain valid.
