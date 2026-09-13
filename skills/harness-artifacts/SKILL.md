---
name: harness-artifacts
description: Produce and validate structured Ticket, Plan, Task Result, Verification Result, and Review Result artifacts.
---

# Harness Artifacts

Use the artifact contract as an interface between roles.

- Return the required artifact type for the current stage, not a mixed narrative.
- Preserve stable identifiers, slice scope, dependencies, acceptance criteria,
  status, and recovery metadata when applicable.
- Tie material claims to exact evidence: command, file, diagnostic, source, or
  host result.
- Use the canonical status vocabulary (`verified`, `unknown`, `blocked`) for
  discovery and native verification.
- Never turn unknown or blocked evidence into approval or a success claim.
- Keep user-facing summaries concise, but do not omit unresolved concerns.
