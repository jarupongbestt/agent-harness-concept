---
name: review-animations
description: Review changed motion in the approved frontend diff as Reviewer. Load only when the assigned changes contain animation, transitions, or gesture-driven motion relevant to review.
---

# Review Animations

Read the [motion review criteria](references/motion-review.md). Check the
changed interaction against its purpose, frequency, project motion tokens,
and reduced-motion behavior. Inspect the code and, where available and
authorized, observe the interaction using relevant input methods and settings.

Remain read-only. Return actionable findings in the standard Review Result
with code locations or concrete interaction steps, user impact, and evidence.
Separate issues shown by the source from questions that need runtime evidence.
Use the harness criteria to determine review status and severity.

Adapted from Emil Kowalski's [review-animations](https://github.com/emilkowalski/skills/tree/64eb3deb9a5fbfa041faf783e0bc3fd635872e9c/skills/review-animations)
skill under its [MIT license](LICENSE).
