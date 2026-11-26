# Portable Agent Harness

A platform-neutral concept for coordinating Claude Code, Codex, Pi, OpenCode,
Hermes, and other AI agents through the same plan-first workflow.

The harness has three layers:

```text
Core workflow + role prompts + artifact schemas + policies
                         ↓
              Knowledge base / project memory
                         ↓
      Platform adapters: Claude Code, Codex, Pi, OpenCode, Hermes
```

The canonical role is **Main Agent**, but this is a multi-agent workflow: the Main
Agent coordinates specialist subagents and does not perform every role itself.
[`AGENTS.md`](AGENTS.md) is the portable bootstrap for this repository. `CLAUDE.md`,
OpenCode config, and Hermes configuration are platform adapters that point to the
same bootstrap and map native delegation mechanisms to it.

## Workflow

```text
User Message → Main Agent intent decision
    ├─ informational question / discussion / status → direct answer (minimal read-only lookup)
    └─ Execution Request
    ↓
Intake completes → clarify if needed → Planner completes the full Plan
    ↓
User approves the full Plan
    ↓
Test Engineer / Builder invocations only: ready, non-conflicting work may overlap
    ├─ create/extend: same-slice Test Engineer completes → handoff → Builder
    ├─ none: no Test Engineer stage; retain dependencies and conflict checks
    └─ different slices: recorded independence allows overlap; unknown → serialize
    ↓
all test/build work completes → Verifier → Reviewer
    ↓
Knowledge Curator assesses and retains eligible durable learning → Finalize
```

The Main Agent routes by meaning and context before Intake. Ordinary questions,
explanations, discussion, and status requests get a direct answer, with minimal
read-only lookup when needed, without creating a new run, Ticket, Plan, specialist
invocation, or full lifecycle. Clear action requests, including "can you fix this?",
and substantial explicitly requested audits or research enter execution. During
active authorized work, answer questions and resume the same run with its artifacts;
questions alone do not cancel, restart, or re-enter stages. Changed requirements
follow the existing re-entry triggers and material Plan approval rules.

"Ok" after a factual explanation acknowledges it without edits. After a concrete
actionable proposal, it selects the approach and advances necessary authorized
Intake and Planning, with edits still waiting for approval of the complete Plan.
After an actual approval request for a presented complete Plan, it explicitly
approves that Plan; continue without asking again.

The execution workflow uses specialist subagents for Intake, Planning, Test Engineering,
Implementation, Verification, Review, and Knowledge Curation. Intake
must finish before Planner starts, and Planner completes the full Plan before
approval or implementation. Only Test Engineer and Builder invocations can
overlap. For `test_subtask.action: create` or `extend`, a same-slice Builder always
waits for completed Test Engineering and its delivered handoff, even with disjoint
files. The handoff identifies the slice, role, approved scope, changed test paths,
criteria coverage, and execution results or an explicit not-run statement.
Completed authoring may include expected red tests for missing behavior; failed,
blocked, or incomplete authoring does not release the Builder. Passing tests are
not a prerequisite for starting implementation. `action: none` creates no Test
Engineer stage and retains ordinary approved dependencies and conflict checks.

Different slices may overlap only when approved, ready, and supported by a
recorded independence assessment of scopes, read/write file use, shared helpers,
fixtures/databases, resources, mutable state, and ordering. In the default
`in_place_uncommitted` shared worktree, assess whether tests/checks can observe
another slice's unfinished changes. Task IDs, separate outputs, or disjoint writes
alone do not prove independence. Serialize affected invocations with a recorded
reason when independence/conflicts are unknown; proven independent work can
overlap without mandatory worktrees. Each invocation and result keeps its approved
slice, role, scope, and handoff attribution, with separate criteria and
responsibility. Verification, Review, and Knowledge Curation wait until
test/build work finishes and do not overlap other specialist work. Root-cause
analysis is a reusable skill loaded by a role when the task requires it; it is
not a permanent agent role.

Ordinary implementation, test, lint, build, and verification failures retry only
the affected Builder slice within its bounded budget, then escalate; they never
restart Intake or Planning. Planning re-enters only for explicit human feedback
or objective evidence that its assumptions are invalid or materially changed.
Material plan changes require renewed approval.

The canonical role-to-skill map is in [`AGENTS.md`](AGENTS.md). A role prompt
defines responsibility; a skill defines a reusable method and is loaded only
when the map's trigger applies. The portable role is **Reviewer**; `Critic` is
the equivalent name used by `template-harness`. The Test Engineer creates or
extends tests from approved acceptance criteria. `harness-artifacts` is a
supporting adapter reference, while artifact requirements are defined by
[`schemas/artifacts.md`](schemas/artifacts.md).

The Main Agent uses a short, meaning-preserving communication policy for everyday
messages. For long or complex explanations, user-facing Plan presentations, and
reports, see the portable
[`human-readable-communication` skill](skills/human-readable-communication/SKILL.md).

When applying the harness, materialize only the skills needed by selected roles
and capabilities, with exactly one definition per method. The three direct
upstream candidates (`context-engineering`, `test-driven-development`, and
`doubt-driven-development`) use upstream definitions only after compatibility review;
local definitions are recorded fallbacks. Other skills follow local defaults
and optional-method mappings. Resolve the selected source, full commit SHA,
access date, and supporting assets before approval. If adapting the skill body
is required to honor harness contracts, record it as a local adaptation.
Unsupported hosts need an approved translation or explicit omission; silent
source substitution is not allowed. See [`APPLY.md`](APPLY.md) and
[`skills/README.md`](skills/README.md) for the complete procedure.

`security-and-hardening` is one conditional local adaptation for Planner, Test
Engineer, Builder, and Reviewer. Ordinary scope/planning information or the actual
diff must identify a concrete affected data, permission, or action boundary and
operation, including indirect private caches, derived data, and exports. Titles
and security keywords alone neither trigger nor exclude it. The
[canonical role triggers](AGENTS.md#canonical-role-to-skill-map) add no mandatory
per-task screening, new specialist, gate, or universal loading; a full audit needs
separately approved scope. Resolve the selected local source's actual full commit
SHA, access date, compatibility, and assets before application approval. Retain
its required boundary reference and license, and rewrite links; the pinned
Cloudflare influence is provenance rather than the local adaptation's revision.
See [source and asset decisions](skills/origins.md#security-and-hardening-adaptation).

The optional [`pr` skill](skills/pr/SKILL.md) is selected independently of the full
workflow or Main Agent. `open` publishes selected changes and returns the PR URL;
`ship` adds current independent review and check evidence, with repairs governed
by existing approved roles and scope. Development still ends at local Finalize,
and Git-action defaults stay disabled. See the
[canonical PR policy](SPEC.md#optional-pr-capability)
and [adapter invocation evidence](adapters/README.md#optional-pr-mapping) before
projecting a native entrypoint; these sources install no runtime command.

`documentation-and-adrs` uses its single local definition as a `local_adaptation`,
with historical knowledge-base attribution preserved; it is not an upstream
candidate or fallback. Applications still resolve the chosen source's full commit
SHA, access date, compatibility, and supporting assets before approval. Materialize
required ADR templates and knowledge rules into approved native destinations,
rewriting links. Map applicable final-write integrity checks to a compatible existing
target checker, translated checks, or the optional reference checker. Selecting the
reference checker also requires its Python runtime, target-specific classification
layout, and check command. Inventory included, translated, referenced, and omitted
assets with reasons; rewrite or remove projected links. Illustrative ADR, log, and
layout assets need not be copied. Required failed or unavailable checks remain
`blocked`. Do not copy the entire example knowledge tree.
Canonical bootstrap and role instructions retain retrieval and final-write check
rules even when a compatible upstream context-engineering skill is selected.

The full workflow includes knowledge management automatically. After review,
the Knowledge Curator assesses every run and retains useful, evidence-backed new
knowledge in the approved, configured system. A reasoned `no_change` is valid when
there is no eligible durable learning; missing configuration or failed writes or
checks are `blocked` with remaining work. Finalization reports a Knowledge Outcome
with its reason, because an empty `knowledge_updates` list cannot show assessment.
The knowledge tree in this repository is an example, not a default destination
for run-specific learning. Partial adoption can select knowledge management alone
without agents, model choices, or run-state storage; a partial scope without it
reports `outside_scope`. No separate per-run opt-in applies to the full workflow.
Ordinary conversation is not a full-workflow execution run and does not invoke
knowledge curation for each message.

The [record-boundary policy](SPEC.md#documentation-knowledge-and-run-artifacts)
keeps three responsibilities distinct:

| Record | Conceptual example |
|---|---|
| Project documentation | Concepts, PRDs, specifications, architecture, and guides remain in their canonical homes under their owners' approved scope. |
| Durable knowledge | A distinct, reusable integration constraint or procedure cites supporting evidence and relevant canonical documents through `source_refs`. |
| Run artifacts | Tickets, Plans, task results, checks, and progress record one execution in its approved run home. |

Placement follows purpose and suitable approved existing configuration, not a
folder name. Curation cannot silently revise requirements or grant general
project-documentation write permission. Ordinary documents do not inherit
knowledge metadata from their topic or location.

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
  the included `knowledge/main.md`, `knowledge/log.md`, and domain
  template/example document this repository's own example layout. They are not
  required paths for projects that adopt the harness.
- [`scripts/validate_knowledge.py`](scripts/validate_knowledge.py) — optional portable
  reference checker, using Python 3.10+ standard library only. Its bounded YAML
  reader and limits are documented in [`knowledge/README.md`](knowledge/README.md).
- [`tests/test_validate_knowledge.py`](tests/test_validate_knowledge.py) — maintainer
  regression tests for the provided checker. Workflow adoption does not automatically
  create, run, or copy these tests into a target project.

This repository supplies reusable workflow, role, skill, and contract sources,
illustrative knowledge assets, and an optional portable checker with maintainer
tests. Platform-specific orchestration implementations belong in adapter
directories; the portable checker is shared reference tooling. Consumers select
or translate the approved sources into discovered native destinations. Python is
required only when selecting this checker, not for every harness adoption.

Applying the concept to another repository does not install this tree. The
applying agent must discover the target platform and project its responsibilities
into that platform's native destinations. For example, a Codex target may use
root `AGENTS.md`, `.codex/agents/`, and `.agents/skills/`, while a Claude Code
target may use `CLAUDE.md`, `.claude/agents/`, and `.claude/skills/`. These are
discovery-verified destinations, not a reason to copy this repository's
`prompts/`, `schemas/`, `knowledge/`, or `skills/` directories wholesale.
