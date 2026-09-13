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
delegate Intake (once by default) → Clarify if needed → delegate Planner (once by default)
    ↓
User Approval
    ↓
Dependency-aware execution graph
    ├─ ready Slice A → Test/Implement → Verify → Review
    ├─ ready Slice B → Test/Implement → Verify → Review
    └─ dependent or conflicting slices wait or serialize
    ↓
delegate Knowledge Update → Finalize
```

The workflow uses specialist subagents for Intake, Planning, Test Design,
Implementation, Review, and Knowledge Curation. Root-cause analysis is a reusable
skill loaded by Intake, Planner, Implementer, or Reviewer when the task requires
it; it is not a permanent agent role. Intake and Planning are single-pass by
default. Intake re-enters only for changed requirements or a newly discovered
Ticket mismatch; Planning re-enters only for explicit human feedback or evidence
that the Plan is invalid or materially changed. Ordinary implementation or test
failures do not restart either stage.

After explicit user approval, the Main Agent schedules dependency-ready slices in
parallel when their approved scopes, files, resources, and mutable state do not
conflict. Dependencies, overlapping files, shared resources, conflicting state,
and ordering requirements serialize only the affected work. “One approved task
slice at a time” means one slice per Implementer invocation, not global
serialization.

An ordinary test or implementation failure routes directly to the same
Implementer slice for a bounded retry with failure evidence and root-cause
context. Re-planning occurs only when evidence shows an invalid Plan or a
material change to scope, acceptance criteria, dependencies, permissions, or
platform assumptions. A materially changed Plan requires renewed user approval
before implementation resumes. Role boundaries, scoped permissions, and the
approval gate remain in force throughout parallel execution.

## Files

- [`SPEC.md`](SPEC.md) — architecture and workflow contract.
- [`APPLY.md`](APPLY.md) — protocol for applying the concept to another project.
- [`schemas/artifacts.md`](schemas/artifacts.md) — portable input/output contracts.
- [`prompts/`](prompts) — role prompt templates.
- [`adapters/README.md`](adapters/README.md) — requirements for agent-platform adapters.
- [`skills/README.md`](skills/README.md) — reusable skill model and materialization
  rules.
- [`AGENTS.md`](AGENTS.md) — multi-agent bootstrap and delegation contract.
- [`skills/origins.md`](skills/origins.md) — origin and provenance of every listed skill.
- [`knowledge/README.md`](knowledge/README.md) — knowledge-base integration rules;
  `knowledge/main.md`, `knowledge/log.md`, and the domain template/example are
  included as a starting structure.

This repository is a concept/specification. Runtime implementations belong in the
platform-specific adapter directories.

Applying the concept to another repository does not install this tree. The
applying agent must discover the target platform and project its responsibilities
into that platform's native destinations. For example, a Codex target may use
root `AGENTS.md`, `.codex/agents/`, and `.agents/skills/`, while a Claude Code
target may use `CLAUDE.md`, `.claude/agents/`, and `.claude/skills/`. These are
discovery-verified destinations, not a reason to copy this repository's
`prompts/`, `schemas/`, `knowledge/`, or `skills/` directories wholesale.
