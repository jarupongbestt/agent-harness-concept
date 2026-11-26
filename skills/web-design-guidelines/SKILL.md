---
name: web-design-guidelines
description: Review changed browser-facing UI against the vendored Vercel Web Interface Guidelines. Load only when the Reviewer is assigned guideline checks for concrete web UI changes.
---

# Web Design Guidelines

Review the affected interface and relevant surrounding code using the
[Rules section](references/web-interface-guidelines.md#rules) of the local
reference. Follow the harness role and Review Result for scope and reporting.

1. Identify relevant rules from the approved diff and affected behavior. Apply
   platform and framework examples to the implementation in context. For
   example, native controls already provide keyboard interactions, and syntax
   such as `onKeyDown` is relevant to custom controls that need that behavior.
2. Check observable semantics, keyboard and pointer interaction, states,
   content, layout, or performance where the change affects them. Confirm
   findings against the approved requirements and local conventions. Mark
   runtime-dependent behavior as unverified when the evidence is source-only.
3. Return the normal Review Result with concrete locations, user impact, and
   evidence for actionable findings. Keep inspection read-only and within the
   assigned changes.

The reference preserves the original bytes of Vercel's pinned
[command.md](https://github.com/vercel-labs/web-interface-guidelines/blob/434b7f91364665f2f733b310ec54809bf8f37937/command.md).
The harness supplies the wrapper and review contract. See the [MIT license](LICENSE).
