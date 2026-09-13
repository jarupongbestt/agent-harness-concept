# Skill Model

This repository is a portable synthesis, not a bundle of skills copied from one
agent runtime. It is also a source library: every listed reusable skill has an
actual `skills/<name>/SKILL.md` definition that an adapter can translate into a
native project skill. The source and provenance of every skill in this catalog is
recorded in [`origins.md`](origins.md). Read that registry before reusing or
extending a skill, and add a provenance entry for each new one.

Roles define **who is responsible** for a stage. Skills define **how the work is
performed**. Skills are reusable across roles and platforms.

## Core skills

| Skill | Used by | Purpose | Origin |
|---|---|---|---|
| context-engineering | Main Agent, Intake, Planner | Keep context focused and progressive | Adapted from template-harness |
| specification | Intake, Planner | Convert intent into checkable criteria | Synthesized from template-harness + requirements practice |
| clarification | Main Agent, Intake | Detect and resolve ambiguity | Adapted from template-harness |
| root-cause | Intake, Planner, Implementer, Reviewer | Trace failures to evidence-backed causes | Adapted from template-harness investigation workflow |
| task-decomposition | Planner | Split work into safe slices | Adapted from template-harness |
| source-driven-development | Intake, Planner | Ground decisions in project and external sources | Adapted from knowledge-base |
| incremental-implementation | Implementer | Make small, reversible changes | Adapted from template-harness |
| test-driven-development | Test Designer, Implementer | Define behavior and make tests pass | Synthesized from template-harness + test-design practice |
| code-review | Reviewer | Inspect correctness, scope, and maintainability | Synthesized from template-harness + code-review practice |
| security-and-hardening | Reviewer, Implementer | Review sensitive boundaries and inputs | Synthesized from template-harness + secure-development practice |
| documentation-and-adrs | Knowledge Curator | Record decisions and durable knowledge | Adapted from knowledge-base |
| harness-artifacts | Main Agent, all artifact-producing roles | Produce and validate portable Ticket, Plan, Task Result, Verification Result, and Review Result artifacts | Synthesized in this harness |
| knowledge-base | Main Agent, Intake, Planner, Knowledge Curator | Navigate, update, and lint the durable knowledge tree | Adapted from knowledge-base |

`root-cause` is intentionally a skill, not a standalone agent. A role loads it
only when the task needs investigation.

Domain skills can be added without changing the lifecycle:

```text
frontend-ui
api-design
database-migrations
data-pipelines
cloud-infrastructure
observability
```

Domain skills are project-specific extensions unless their own source is recorded;
their default origin is not this repository. See [`origins.md`](origins.md) for
stable links and the full provenance rules.

## Materialization rule

An adapter must use the actual definitions under this directory, not this catalog
alone. For Codex, each selected definition becomes
`.agents/skills/<name>/SKILL.md`; for Claude Code, it becomes
`.claude/skills/<name>/SKILL.md`. The adapter may select all core skills or a
smaller approved set. For a full harness application, the default is every entry
listed in `harness.yaml` under `skills.definitions`; a smaller set needs explicit
omission reasons. It must not claim a skill was installed when its source
definition is absent. Conditional invocation, such as `root-cause`, does not mean
conditional installation.
