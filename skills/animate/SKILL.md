---
name: animate
description: Implement purposeful motion for an approved frontend interaction as Builder. Load only when the assigned slice explicitly includes adding or changing motion.
---

# Animate

Use the [motion guidance](references/motion-guidance.md) after identifying the
affected interaction, existing motion tokens, expected input methods, and
reduced-motion behavior.

1. Decide whether motion helps users understand a state change, locate a
   surface, or get feedback. Keep routine repeated actions and fast keyboard
   flows immediate when animation would delay the task. If the approved scope
   requires a particular effect, surface a material conflict to the Main Agent.
2. Choose the simplest supported mechanism and reuse existing design-system
   tokens and primitives. Specify the animated properties, origin, timing,
   and how motion handles reversal, dismissal, and reduced-motion preferences.
   Keep the underlying interaction, focus behavior, and state usable throughout.
3. Verify the changed interaction with available, authorized evidence. Check
   rapid repetition or reversal, relevant keyboard and pointer behavior, and
   the reduced-motion path. Report observed results and limits in the Task
   Result.

The approved slice and harness role and artifact contracts govern the work.
Adapted from Emil Kowalski's [animate](https://github.com/emilkowalski/skills/tree/64eb3deb9a5fbfa041faf783e0bc3fd635872e9c/skills/animate)
skill under its [MIT license](LICENSE).
