---
name: context-engineering
description: Keep agent context small, role-specific, and sufficient for the current decision or task slice.
---

# Context Engineering

Build the smallest context that can support the current decision.

- Start with the Ticket, approved scope, relevant instructions, and matching
  knowledge navigation; do not preload unrelated repository areas.
- Prefer summaries, exact file references, and evidence links over large pasted
  file contents.
- Pass each specialist only its role, task slice, dependencies, relevant
  knowledge, and required output contract.
- Preserve unresolved assumptions and evidence gaps instead of filling them with
  guesses.
- Re-read source files when a decision depends on details that were summarized.

Return distilled state that another role can use without the full transcript.
