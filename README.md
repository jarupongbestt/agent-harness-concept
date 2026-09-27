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
Knowledge Curator when explicitly selected → Finalize
```

The workflow uses specialist subagents for Intake, Planning, Test Engineering,
Implementation, Verification, and Review. Knowledge Curation is optional. Intake
must finish before Planner starts, and Planner completes the full Plan before
approval or implementation. Only Test Engineer and Builder invocations can
overlap, and only when dependencies are ready and scopes, files, resources,
mutable state, and ordering do not conflict. A Builder waits for test output it
depends on. Verification, Review, and any selected Knowledge Curation wait until
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

Knowledge curation and durable knowledge writes occur only when the user selects
knowledge management or explicitly requests durable learning. The knowledge tree
in this repository is an example, not a default destination for run-specific
learning. When the capability is omitted, an empty `knowledge_updates` result is
valid.

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
