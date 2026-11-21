---
name: code-review
description: Independently review a change for correctness, scope, regressions, maintainability, and test quality.
---

# Code Review

Review against the Ticket, approved Plan, acceptance criteria, and verification
evidence—not against personal implementation preference.
Apply [code-quality](../code-quality/SKILL.md) with review depth proportional to
the scope and risk.

- Check missing behavior, edge cases, regressions, and error handling.
- Confirm changed files stay within approved scope.
- Inspect tests for meaningful behavior rather than implementation tautologies.
- Check maintainability and compatibility with existing project conventions.
- Check that control flow, data flow, and state changes remain readable and easy
  to trace.
- Assess cohesive responsibilities in the resulting changed files, including
  large files; use no hard size threshold or automatic splitting rule.
- Look for duplication of logic, state, or helpers with concrete maintenance or
  correctness impact; do not mandate an abstraction or pattern.
- Check for code made unused by this change using scoped inspection and available
  Verifier evidence. State evidence limits rather than claim unused code without
  support, and do not expand into broad cleanup.
- Examine security and data handling when the change crosses a sensitive boundary.
- Separate observed evidence from assumptions; state uncertainty and request the
  smallest missing evidence needed to support a finding.
- Order findings by severity and include concrete file/line evidence and impact.
  Distinguish required fixes from optional preferences; zero findings is valid.

Do not rewrite the implementation while reviewing it.
