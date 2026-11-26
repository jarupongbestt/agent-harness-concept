# Design Criteria

## Builder

- Build for the primary task on this surface. A dashboard should be easy to
  scan and operate. A guide should be easy to read. A landing page should make
  the offer and next action clear. Use composition, spacing, type, and color
  to make that priority apparent.
- Preserve the visual identity in a refinement. For an approved new direction,
  let the brief, actual content, and audience drive the choices. Use tokens,
  assets, and components already in use where they support the intended result.
  Add visual emphasis and text only where they give the user useful information
  or a clear affordance.
- Test the hierarchy with realistic content and the relevant narrow and wide
  layouts. Check contrast, readable line lengths, safe wrapping, and grouping.
  Check the states the flow actually reaches, such as visible keyboard focus,
  selected, disabled, loading, empty, and error. Use the product's language for
  labels and recovery instructions. Keep motion purposeful and respect reduced
  motion when it applies.

## Reviewer

- Walk the primary task. Can users recognize where they are, find the next
  action, tell what changed, and recover from errors? Look for attention spent
  on decoration that competes with the task, missing visual cues, or patterns
  that diverge from comparable screens without serving the brief.
- Use current preview evidence or captures when available within scope. Check
  relevant viewport, content, theme, and interaction variants with the largest
  impact on the task. Cite the route or component, state, viewport when
  relevant, and the actual difference that affects users. Source alone can
  support a finding about a rule or state, while some judgments about visual
  hierarchy and fit need a render.
- Prioritize findings by user impact in the Review Result. Anchor aesthetic
  judgments to the brief, design system, or a specific usability consequence.
  Keep implementation with the Builder.

Adapted from Impeccable's [craft floor](https://github.com/pbakaus/impeccable/blob/2fc5e1459efbbf06d92c7306517c286bf12490cf/.agents/skills/impeccable/reference/craft-floor.md)
and [critique](https://github.com/pbakaus/impeccable/blob/2fc5e1459efbbf06d92c7306517c286bf12490cf/.agents/skills/impeccable/reference/critique.md).
