---
name: doubt-driven-development
description: Cross-examine a non-trivial decision with an adversarial reviewer in a fresh context before committing to it. Use when stakes, uncertainty, or verification cost justify an independent challenge.
---

# Doubt-Driven Development

This is an orchestration method for the Main Agent, not a Reviewer persona.
Use it before a consequential decision is treated as settled; it complements
final review by making course correction possible while the work is still
underway.

1. **State the claim.** Write the decision or proposed conclusion and the key
   evidence and assumptions behind it.
2. **Extract the smallest unanchored review packet.** Keep your claim and
   hypothesis private. Send a fresh-context Reviewer only the smallest relevant
   artifact, its applicable contract, and necessary source references. Ask the
   Reviewer to independently find errors, missing cases, and ways the artifact
   could fail; do not reveal or hint at the conclusion you want checked.
3. **Reconcile findings.** Check each challenge against evidence. Correct the
   claim or record why a challenge does not apply; do not accept criticism solely
   because it sounds confident.
4. **Stop deliberately.** Use at most one focused follow-up review when a
   material issue remains. Then proceed, revise, or surface the unresolved issue
   to the user with evidence.

Apply this selectively to non-trivial, high-stakes, unfamiliar, or hard-to-verify
decisions. Skip routine mechanical checks and obvious low-risk choices. A fresh
context is required; a particular model provider or CLI is not. Any cross-model
or external service escalation requires separate user authorization.

This method is distinct from `root-cause`, which investigates an observed
failure, `source-driven-development`, which verifies claims against sources, and
`code-review`, which assesses a completed change.
