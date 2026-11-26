# Motion Guidance

Match the amount of motion to its purpose and frequency. Press feedback can
be quick and subtle. Moving a panel or explaining an unfamiliar transition can
justify more movement. Repeated shortcuts and fast list navigation usually
benefit from an immediate state change.

## Mechanism and properties

- Use a transition for interactive state changes that may reverse mid-flight.
  Use a CSS animation for a fixed sequence. Use the existing platform's
  programmatic animation primitive or existing project motion library when user
  control requires cancellation, coordinated exits, or a gesture that can
  change direction. Prefer the simplest supported option in the project.
- Prefer `transform` and `opacity` for moving elements and list the transitioned
  properties explicitly. Consider a measured height for an accordion only when
  content reflow is part of the behavior. Keep layout work and expensive paint
  effects out of frequently repeated motion where possible.
- Scale a popover, menu, or tooltip from the actual trigger; a centered dialog
  can keep a centered origin. When scaling an entering surface, a small offset
  such as `scale(0.95)` with opacity preserves a sense of continuity.

## Timing and input

Reuse product tokens first. As starting points when the project has none, try
100–160 ms for press feedback, 125–200 ms for a tooltip or small popover, and
150–250 ms for a dropdown. Verify larger surfaces and explanatory sequences in
context. A responsive entrance often suits an ease-out curve such as
`cubic-bezier(0.23, 1, 0.32, 1)`. Movement between two stable positions can use
an ease-in-out curve. Tune timing to the interaction and keep the action ready
while decorative motion plays.

Put optional hover motion behind `(hover: hover) and (pointer: fine)` where
appropriate. Provide a `prefers-reduced-motion: reduce` variant on the web or
the host's equivalent. Remove nonessential travel, scaling, parallax, and
loops in that variant. Keep useful feedback through an immediate change or a
calm opacity or color transition. Make the final state and focus behavior
equivalent across preferences.

Adapted from Emil Kowalski's [build sequence](https://github.com/emilkowalski/skills/blob/64eb3deb9a5fbfa041faf783e0bc3fd635872e9c/skills/animate/SKILL.md)
and [recipes](https://github.com/emilkowalski/skills/blob/64eb3deb9a5fbfa041faf783e0bc3fd635872e9c/skills/animate/RECIPES.md).
