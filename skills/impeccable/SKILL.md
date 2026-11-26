---
name: impeccable
description: Apply product-specific design craft to approved frontend UI changes as Builder, or critique visual quality in the approved UI diff as Reviewer. Load only when the assigned scope calls for visual design or visual critique.
---

# Impeccable

Use the [design criteria](references/design-criteria.md) for the assigned user
flow. Start with the approved brief and the design system already in use. Read
the relevant components, tokens, copy, and available visual references before
deciding which criteria matter to this surface.

- **Builder.** Make the authorized design change using existing patterns and
  tokens where they fit. Verify its important states and viewport sizes with
  available, authorized evidence. Record what was observed and any limits in
  the Task Result.
- **Reviewer.** Critique the changed surface against the brief and incumbent
  design. Keep the work read-only and report concrete issues, their user impact,
  and supporting locations or UI states in the Review Result. Distinguish code
  observations from rendered behavior and call out unobserved states when they
  matter to a finding.

The harness defines role boundaries, approved scope, and artifact requirements.
The method can use an existing preview or captures when available. It works from
the brief and source with explicitly limited visual evidence when those tools
are unavailable.

Adapted for the harness from [Impeccable](https://github.com/pbakaus/impeccable/tree/2fc5e1459efbbf06d92c7306517c286bf12490cf/.agents/skills/impeccable).
The original [license](LICENSE) and [third-party notices](NOTICE) are retained.
