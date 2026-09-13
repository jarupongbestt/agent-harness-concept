# Portable Agent Harness

A platform-neutral concept for coordinating Claude, Codex, OpenCode, Hermes, and
other AI agents through the same plan-first workflow.

The harness has three layers:

```text
Core workflow + role prompts + artifact schemas + policies
                         ↓
              Knowledge base / project memory
                         ↓
      Platform adapters: Claude, Codex, OpenCode, Hermes
```

The canonical role is **Main Agent**, but this is a multi-agent workflow: the Main
Agent coordinates specialist subagents and does not perform every role itself.
[`AGENTS.md`](AGENTS.md) is the portable bootstrap for this repository. `CLAUDE.md`,
OpenCode config, and Hermes configuration are platform adapters that point to the
same bootstrap and map native delegation mechanisms to it.

## Workflow

```text
User Request
    ↓
Main Agent (orchestrates)
    ↓
delegate Intake → Clarify → delegate Plan
                         ↓
                   User Approval
                         ↓
delegate Test Design → delegate Implement → delegate Verify → delegate Review
                         ↓
           delegate Knowledge Update → Finalize
```

The workflow uses specialist subagents for Intake, Planning, Test Design,
Implementation, Review, and Knowledge Curation. Root-cause analysis is a reusable
skill loaded by Intake, Planner, Implementer, or Reviewer when the task requires
it; it is not a permanent agent role.

## Files

- [`SPEC.md`](SPEC.md) — architecture and workflow contract.
- [`APPLY.md`](APPLY.md) — protocol for applying the concept to another project.
- [`schemas/artifacts.md`](schemas/artifacts.md) — portable input/output contracts.
- [`prompts/`](prompts) — role prompt templates.
- [`adapters/README.md`](adapters/README.md) — requirements for agent-platform adapters.
- [`skills/README.md`](skills/README.md) — reusable skill model.
- [`AGENTS.md`](AGENTS.md) — multi-agent bootstrap and delegation contract.
- [`skills/origins.md`](skills/origins.md) — origin and provenance of every listed skill.
- [`knowledge/README.md`](knowledge/README.md) — knowledge-base integration rules.

This repository is a concept/specification. Runtime implementations belong in the
platform-specific adapter directories.
