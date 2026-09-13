# Applying the Harness to a Project

This document is the implementation protocol for an agent asked to “apply this
agent harness” to another repository. Applying means integrating selected
workflow responsibilities into the target repository's native agent setup. It
does not mean installing this repository.

## Important: this is a concept, not a fixed folder template

This repository is a portable concept and specification, not a package, starter
template, or canonical target-repository layout. The folders and filenames shown
below are **illustrative adapter targets**, not a required directory tree. The
portable concept defines responsibilities, workflow, permissions, artifacts, and
knowledge rules. Each platform and target repository may map those responsibilities
to different paths or native configuration.

Never copy, fork, install, or reproduce the `agent-harness` repository wholesale
in a target repository. In particular, do not automatically copy its root files
or its `adapters/`, `knowledge/`, `prompts/`, `schemas/`, or `skills/` trees.
First inspect the target repository, identify the platform(s) it actually uses,
and create only target-native files and directories needed for the approved setup.
Reuse or adapt an individual concept only when the mapping and approval process
below explicitly permits it.

## Application protocol

When the user asks to apply this harness:

1. Inspect the target repository and identify its agent platform(s), existing
   instruction files, agent/subagent configuration, skills, test commands, and
   knowledge directories. Record an inventory before proposing or writing any
   target files. Existing instructions and configuration are inputs to preserve,
   adapt, or explicitly migrate; they are not a reason to duplicate them.
2. Read this repository's [`AGENTS.md`](AGENTS.md), [`SPEC.md`](SPEC.md), adapter
   requirements, role prompts, artifact contracts, and skill provenance registry
   as source material for translation. Do not treat any source path as a path to
   copy into the target.
3. Create a target-specific concept map before proposing implementation. For
   every relevant portable concept, mark one action: `reuse` an existing target
   mechanism, `translate` the concept into a target-native mechanism, `reference`
   the source without copying it, or `omit` it. Every `omit` entry must include a
   reason, such as unsupported host capability, existing equivalent, or explicit
   user choice. The map must identify the target-native destination for every
   `reuse` or `translate` entry. At minimum, cover workflow/lifecycle, specialist
   roles, artifacts, approval, model policy, permissions, verification, knowledge,
   run state, and provenance. Keep the `reuse`/`translate`/`reference`/`omit`
   action for each covered concept, and record a reason for every omission.
4. Ask the user to choose or confirm:
   - target platform(s), such as Claude, Codex, OpenCode, Hermes, or a combination;
   - whether to preserve or migrate existing agent instructions;
   - the model policy for the Main Agent and each subagent role;
   - whether the user approves creating or changing the knowledge and run-state
     directories.
5. Propose a small platform-specific file map derived from the inventory and
   concept map. Get explicit user approval for both the map and any model,
   knowledge, or run-state decisions before writing, generating, installing, or
   changing anything in the target.
6. After approval, create or change only files listed in the approved target file
   map, plus files required by an explicitly documented platform convention that
   the map names. Do not create a source-repository-shaped tree, unapproved role
   files, or duplicate existing instructions. The bootstrap must tell the host to
   use specialist subagents when supported.
7. Translate or selectively reuse the role prompts and skills named in the map;
   do not copy this concept's example paths blindly.
8. Connect the target project's knowledge base, if it has one, using the
   knowledge read/write contract and only with the approved knowledge scope.
9. Validate that the adapter can delegate, restrict permissions, ask for approval,
   preserve artifacts, run checks, and finalize knowledge updates. Audit the
   resulting diff against the approved map and confirm that the illustrative
   source tree was not copied.
10. Report the exact files created, changed, or left untouched, the concept-map
    omissions and reasons, the model choices, unsupported host capabilities, any
    validation results, and how to start a run.

Do not begin implementation work in the target project while performing this
installation unless the user separately asks for an implementation task.

## Language policy

Keep internal reasoning and all messages exchanged with subagents in English. This
includes delegation prompts, role instructions, structured artifacts, findings,
and task results. English is the portable coordination language of the harness.

Respond to the user in the language of the user's request. If the request is
multilingual, a hybrid response is allowed and should follow the user's language
mix. Keep code, paths, commands, model identifiers, and schema field names
unchanged.

## Illustrative platform mapping

Use the platform's current documentation and capabilities to confirm the actual
paths. These are examples of where an adapter might live:

| Portable responsibility | Claude example | Codex example | OpenCode example | Hermes example |
|---|---|---|---|---|
| Project bootstrap | `CLAUDE.md` | `AGENTS.md` | OpenCode project config | Hermes project/config file |
| Specialist subagents | `.claude/agents/` | `.codex/agents/` or the host's current agent mechanism | `.opencode/agents/` or native agent mechanism | Native Hermes agent/session mechanism |
| Reusable skills | `.claude/skills/` | `.agents/skills/` or the host's current skill mechanism | `.opencode/skills/` or native skill mechanism | Native Hermes skill mechanism |
| Durable knowledge | `knowledge/` | `knowledge/` | `knowledge/` or an approved compatible path | `knowledge/` or an approved compatible path |
| Temporary run state | `.harness/runs/` | `.harness/runs/` | `.harness/runs/` or native session storage | `.harness/runs/` or native session storage |

The `.claude/`, `.codex/`, and `.agents/` paths above are not interchangeable
standards and are not guaranteed to be supported by every version of a platform.
The adapter must verify the host's current convention before creating them.

## What the applying agent should create

Create only the target-native files required to provide the capabilities selected
in the approved map. The following are capability examples, not a mandatory file
list or one-file-per-role layout:

- a platform bootstrap or integration with the target's existing instructions;
- specialist delegation for each required role, using the host's native mechanism;
- role guidance adapted from [`prompts/`](prompts) when the target needs it;
- mappings for the portable skills in [`skills/README.md`](skills/README.md) when
  the target has compatible skill support;
- knowledge integration only when the target uses a knowledge base and the user
  approves its scope;
- run-state storage only when the target needs persisted execution artifacts and
  the user approves it.

The target may implement these capabilities with zero, one, or several native
files. Do not create a separate file for every role, capability, or example path
unless the target platform requires it and the approved map names it. Do not add
files merely to mirror this repository's `prompts/`, `schemas/`, `skills/`,
`knowledge/`, or `adapters/` directories.

Do not create a separate Investigator agent by default. Load `root-cause` as a
skill in the role that needs it unless the target platform has a specific reason to
make investigation a separate subagent.

## Model selection is a user decision

The applying agent must ask about models before generating platform-specific
configuration. Model names, availability, pricing, and reasoning controls change
over time, so this repository must not hard-code a permanent model name.

Ask a question like:

```text
Which current model should the Main Agent and each subagent use?

Recommendation: use the smallest current model that is reliable for the role,
with stronger reasoning for planning, security-sensitive work, and independent
review. I can suggest options from the models currently available in your platform.
For example only—not a permanent default—you might choose a current small/fast
Claude model with high reasoning and a current small/fast Codex model with high
reasoning for low-risk work, then use a stronger model for tier-2 work.

Would you like one model for every role, or a tiered model policy?
```

The user may choose one model for all roles or a tiered policy. A tiered policy is
usually more economical:

| Work | Suggested policy |
|---|---|
| Intake, simple checks, bounded implementation | Smallest reliable current model |
| Planning, cross-cutting implementation | Standard or stronger current model |
| Security, data loss risk, low-confidence work, independent review | Strongest approved current model |

These are routing recommendations, not model identifiers. The adapter should
record the user's actual choices in its configuration and final run summary.
