# Applying the Harness to a Project

This document is the implementation protocol for an agent asked to “apply this
agent harness” to another repository.

## Important: this is a concept, not a fixed folder template

The folders and filenames shown below are **illustrative adapter targets**, not a
required directory tree. The portable concept defines responsibilities, workflow,
permissions, artifacts, and knowledge rules. Each platform and target repository
may map those responsibilities to different paths or native configuration.

Do not create every example directory automatically. First inspect the target
repository, identify the platform(s) it actually uses, and create only the adapter
files and directories needed for the approved setup.

## Application protocol

When the user asks to apply this harness:

1. Inspect the target repository and identify its agent platform(s), existing
   instruction files, agent/subagent configuration, skills, test commands, and
   knowledge directories.
2. Read this repository's [`AGENTS.md`](AGENTS.md), [`SPEC.md`](SPEC.md), adapter
   requirements, role prompts, artifact contracts, and skill provenance registry.
3. Ask the user to choose or confirm:
   - target platform(s), such as Claude, Codex, OpenCode, Hermes, or a combination;
   - whether to preserve or migrate existing agent instructions;
   - the model policy for the Main Agent and each subagent role;
   - whether the user approves creating or changing the knowledge and run-state
     directories.
4. Propose a small platform-specific file map and get approval before writing it.
5. Create the platform bootstrap and subagent role configuration. The bootstrap
   must tell the host to use specialist subagents when supported.
6. Install or adapt the role prompts and skills without copying this concept's
   example paths blindly.
7. Connect the target project's knowledge base, if it has one, using the
   knowledge read/write contract.
8. Validate that the adapter can delegate, restrict permissions, ask for approval,
   preserve artifacts, run checks, and finalize knowledge updates.
9. Report the exact files created or changed, the model choices, unsupported host
   capabilities, and how to start a run.

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

The usual minimum is:

- one platform bootstrap that points to the portable workflow;
- one subagent definition or equivalent isolated configuration for each required
  role: Intake, Planner, Test Designer when needed, Implementer, Verifier,
  Reviewer, and Knowledge Curator;
- role prompts adapted from [`prompts/`](prompts);
- a mapping for the portable skills in [`skills/README.md`](skills/README.md);
- a knowledge integration only when the target uses a knowledge base;
- run-state storage only when the target needs persisted execution artifacts.

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
