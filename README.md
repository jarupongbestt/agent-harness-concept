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
User Message → Main Agent intent decision
    ├─ informational question / discussion / status → direct answer (minimal read-only lookup)
    └─ Execution Request
    ↓
Intake completes → clarify if needed → Planner completes the full Plan
    ↓
User approves the full Plan
    ↓
Test Engineer / Builder invocations only: ready, non-conflicting work may overlap
    ├─ dependent test/build pair runs in order
    └─ dependencies and conflicts serialize affected work
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
overlap, and only when dependencies are ready and scopes, files, resources,
mutable state, and ordering do not conflict. A Builder waits for test output it
depends on. Verification, Review, and Knowledge Curation wait until
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
and capabilities, with exactly one definition per method. The five direct
upstream candidates use upstream definitions only after compatibility review;
local definitions are recorded fallbacks. Other skills follow local defaults
and optional-method mappings. Resolve the selected source, full commit SHA,
access date, and supporting assets before approval. If adapting the skill body
is required to honor harness contracts, record it as a local adaptation.
Unsupported hosts need an approved translation or explicit omission; silent
source substitution is not allowed. See [`APPLY.md`](APPLY.md) and
[`skills/README.md`](skills/README.md) for the complete procedure.

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

This repository is a concept/specification. Runtime implementations belong in the
platform-specific adapter directories.

Applying the concept to another repository does not install this tree. The
applying agent must discover the target platform and project its responsibilities
into that platform's native destinations. For example, a Codex target may use
root `AGENTS.md`, `.codex/agents/`, and `.agents/skills/`, while a Claude Code
target may use `CLAUDE.md`, `.claude/agents/`, and `.claude/skills/`. These are
discovery-verified destinations, not a reason to copy this repository's
`prompts/`, `schemas/`, `knowledge/`, or `skills/` directories wholesale.
