---
name: root-cause
description: Investigate bugs, failed checks, and unexpected behavior using direct evidence before proposing a fix.
---

# Root Cause

Trace the observed symptom to a bounded, evidence-backed cause.

1. Reproduce or inspect the failure with the narrowest useful check.
2. Record the exact input, output, environment, and failing location.
3. Trace the control and data flow toward the earliest incorrect state.
4. Distinguish confirmed facts, likely hypotheses, and unresolved gaps.
5. Propose the smallest fix that addresses the cause and a regression check.

Do not label a failing test as proof that the plan is invalid. Route ordinary
failures back to the same implementation slice; re-plan only when evidence shows
the approved scope or assumptions are invalid.
