---
name: code-review
description: Independently review a change for correctness, scope, regressions, maintainability, and test quality.
---

# Code Review

Review against the Ticket, approved Plan, acceptance criteria, and verification
evidence—not against personal implementation preference.

- Check missing behavior, edge cases, regressions, and error handling.
- Confirm changed files stay within approved scope.
- Inspect tests for meaningful behavior rather than implementation tautologies.
- Check maintainability and compatibility with existing project conventions.
- Examine security and data handling when the change crosses a sensitive boundary.
- Order findings by severity and include concrete file/line evidence.

Do not rewrite the implementation while reviewing it.
