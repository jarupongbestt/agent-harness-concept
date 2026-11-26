# Motion Review Criteria

- **Purpose and frequency.** Identify what the change conveys, such as action
  feedback, a state change, or spatial context. Look for slow or distracting
  motion on repeated actions, including keyboard flows. Consider whether an
  immediate state change or less movement would better serve the user.
- **Timing and origin.** Compare duration and easing to existing tokens and
  similar controls. Look for perceived delay on entrances or exits, a menu
  scaling from the wrong origin, and a surface visually appearing from zero
  scale. Time the visible stages independently when a deliberate hold and a
  quick release serve different purposes. Treat generic timings as starting
  points. A dropdown often feels responsive around 150–250 ms, and a large
  drawer can need more time.
- **Interruptibility.** Try rapid toggling, reverse a gesture, or dismiss an
  entering surface where those actions apply. Inspect whether the code retargets
  from its current state, cancels cleanly, and keeps controls usable when an
  animation is in progress. Check that focus and final state remain consistent.
- **Rendering cost.** Flag a concrete risk when repeated motion drives layout
  or paint unnecessarily or transitions unspecified properties. Prefer
  targeted `transform` and `opacity` where feasible. Evaluate necessary reflow,
  such as an expanding accordion, against the actual behavior. Claims about
  perceived stutter or frame rate need observation or measurement.
- **Access and input.** Check that reduced-motion settings remove or tone down
  movement while retaining meaningful feedback and the same end state. Check
  hover motion on capable inputs and equivalent operation by keyboard and touch
  where the interaction supports them.

Cite the observed state and resulting problem. Suggest the smallest viable
correction supported by the project's tokens and interaction model. When feel
cannot be established from code and a preview is unavailable, record the
coverage limit in the Review Result.

Adapted from Emil Kowalski's [review method](https://github.com/emilkowalski/skills/blob/64eb3deb9a5fbfa041faf783e0bc3fd635872e9c/skills/review-animations/SKILL.md)
and [motion standards](https://github.com/emilkowalski/skills/blob/64eb3deb9a5fbfa041faf783e0bc3fd635872e9c/skills/review-animations/STANDARDS.md).
