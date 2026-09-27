---
name: karpathy-guidelines
description: Keep coding work simple, assumption-aware, narrowly scoped, and verifiable. Use for implementation and review to prevent overengineering and wasted effort.
---

# Karpathy Guidelines

Use these principles to make the smallest reliable change and avoid spending
effort or context on work that does not satisfy the approved goal.

1. **Think before acting.** Surface assumptions and tradeoffs. Ask when different
   interpretations would materially change the result.
2. **Choose the simplest sufficient solution.** Avoid speculative features,
   abstractions, configuration, and defensive code without a demonstrated need.
3. **Keep edits surgical.** Change only what the request and acceptance criteria
   require. Preserve nearby behavior and conventions; do not make unrelated
   cleanup changes.
4. **Define and verify the outcome.** Use observable acceptance criteria and the
   narrowest useful checks. Report what could not be verified.

For trivial, unambiguous work, apply the principles proportionately. Simplicity
does not justify omitting required behavior, tests, or safety controls.
