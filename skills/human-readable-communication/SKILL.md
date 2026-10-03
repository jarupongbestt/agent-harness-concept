---
name: human-readable-communication
description: Make the Main Agent's long or complex explanations, user-facing Plan presentations, and reports easier to follow while preserving meaning and the user's language. Short messages use the bootstrap policy.
license: MIT; see LICENSE
---

# Human-Readable Communication

Help the user understand the outcome, action, or decision without changing what
the source says. Apply this method to the Main Agent's user-facing presentation
for long or complex explanations, Plan presentations, and reports.
Short messages use the bootstrap's lightweight policy.

## Preserve the contract and meaning

- Keep internal coordination and specialist artifacts in English. This skill
  does not automatically rewrite their artifacts or change roles, required
  fields, approval gates, permissions, or lifecycle stages.
- Retain facts, numbers, units, negation, exceptions, evidence, confidence,
  uncertainty, and obligations. Permission (`may`) is not a requirement (`must`)
  or a recommendation (`should`); preserve those distinctions in translation.
- Keep technical names, code, paths, commands, quoted text, and artifact field
  names exact. Explain them in surrounding prose when the user needs context.
- Do not add facts, promises, causes, or conclusions merely to improve flow.
  Readability does not guarantee that the underlying content is true.

## Shape the presentation

Lead with the outcome, action, or decision the user needs. Then explain the
reason, evidence, and next step at the level needed to assess it.

- Use familiar words and make clear who acts, decides, or is affected.
- Keep each paragraph focused on a topic; connect details in a useful order.
- Use stable terms for the same concept. Avoid synonyms that obscure whether
  two phrases refer to the same thing.
- Place each condition or exception beside the action it governs. Keep the
  scope of negation and qualifying statements clear when splitting sentences.
- Prefer direct verbs and remove repetition when it carries no extra meaning.
- Use lists, tables, or diagrams when they make steps, comparisons, or
  relationships easier to follow; choose prose when it works better.
- Include enough context for a decision or approval. A shorter explanation
  must not hide scope, dependencies, risks, evidence, or unresolved questions.

## Follow the user's language and tone

Use the language the user uses, a natural mix when they mix languages, and their
preferred tone. Keep the wording natural for that language and audience.

For Thai, make the actor, action, condition, and exception clear through natural
phrasing and paragraph structure. Keep English technical terms when appropriate.
Do not impose an English dictionary, grammar rules, or word-count targets on
Thai or mixed-language prose.

For English, the upstream 20-word instruction and 25-word description targets
can prompt a second look at a difficult sentence; they are optional editing
prompts, not caps. Keep a longer sentence, passive voice, or another grammatical
form when it is more accurate or natural. An informal "80%" clarity aim is not
a measured score or an ASD-STE100 compliance claim.

## Check before sending

Compare the presentation with its source or structured artifact. Confirm that
the actor, action, conditions, numbers, evidence, uncertainty, and obligations
still mean the same thing. Check protected names and text for exact preservation.
If a simpler wording changes meaning, keep the accurate wording and explain it.
Use editorial judgment; do not add strict mode, a linter, or automatic rewriting.

## Source and license

This is a local adaptation influenced by
[danyuchn/asd-ste100-skill v0.4.0](https://github.com/danyuchn/asd-ste100-skill/blob/7d4a135a199a5d7447c4886bcd7ffe742a627bc9/SKILL.md),
commit `7d4a135a199a5d7447c4886bcd7ffe742a627bc9`, accessed 2026-10-02.
The pinned source was reviewed; strict English rules and upstream examples and
linter are intentionally omitted. The method above is self-contained.
See [../origins.md](../origins.md) for asset decisions and
[LICENSE](LICENSE) for the retained MIT copyright and permission notice.
