# Skill Model

This repository is a portable synthesis, not a bundle of skills copied from one
agent runtime. It is also a source library: every listed reusable skill has an
actual `skills/<name>/SKILL.md` definition that an adapter can translate into a
native project skill. The source and provenance of every skill in this catalog is
recorded in [`origins.md`](origins.md). Read that registry before reusing or
extending a skill, and add a provenance entry for each new one.

Roles define **who is responsible**, their boundaries, and required artifacts.
Those instructions belong in role prompts. Skills define reusable methods an
agent loads when a mapping or task trigger calls for them; they are not a second
copy of the role prompt.

## Core skills

| Skill | Role mapping | Trigger / purpose | Origin |
|---|---|---|---|
| context-engineering | Main Agent, Planner | Required for orchestration and planning; keep context focused and progressive | Adapted from template-harness |
| specification | Intake | Required for intake; turn intent into checkable criteria | Synthesized from template-harness + requirements practice |
| clarification | Main Agent, Intake | Required; interview/refine only as needed to resolve material ambiguity | Adapted from template-harness; interview and idea-refinement techniques synthesized |
| root-cause | Intake, Planner, Implementer, Reviewer | Conditional for bug/failure investigation; Planner uses it when Intake evidence is incomplete, contradictory, or insufficient | Adapted from template-harness investigation workflow |
| task-decomposition | Planner | Required; split approved work into safe, verifiable slices | Adapted from template-harness |
| source-driven-development | Intake, Planner | Required; ground decisions in project and external sources | Adapted from knowledge-base |
| incremental-implementation | Implementer | Required; implement one approved slice with bounded scope | Adapted from template-harness |
| test-driven-development | Test Engineer | Required when tests are authored or changed; derive behavior from criteria | Synthesized from template-harness + test-design practice |
| code-review | Reviewer | Required; inspect correctness, scope, regressions, and maintainability | Synthesized from template-harness + code-review practice |
| security-and-hardening | Reviewer, Implementer | Conditional when work crosses sensitive boundaries | Synthesized from template-harness + secure-development practice |
| documentation-and-adrs | Knowledge Curator | Required for every full-workflow knowledge assessment and included partial knowledge scope | Adapted from knowledge-base |
| knowledge-base | Main Agent, Intake, Planner, Knowledge Curator | Required for full workflow and partial scopes including knowledge; use the configured system | Adapted from knowledge-base |
| karpathy-guidelines | Implementer, Reviewer | Required; surface assumptions, minimize complexity, keep edits surgical, verify outcomes | Synthesized from Karpathy's published observations and portable skill practice |
| doubt-driven-development | Main Agent only | Conditional for non-trivial, high-stakes, unfamiliar, or hard-to-verify decisions; fresh-context adversarial challenge | Adapted from addyosmani/agent-skills; orchestration details made portable |
| [human-readable-communication](human-readable-communication/SKILL.md) | Main Agent only | Conditional for long or complex explanations, user-facing Plan presentations, and reports; preserve meaning while making the presentation easier to follow | Local adaptation of danyuchn/asd-ste100-skill v0.4.0; relaxed and language-aware |

`root-cause` is a reusable method, not a standalone agent. The same principle
applies to all skills: the role prompt owns the stage and the skill supplies a
method only when its trigger applies. `doubt-driven-development` belongs to the
Main Agent's orchestration and must not be placed in the Reviewer persona.

`human-readable-communication` guides the Main Agent's user-facing presentation.
Short messages use the bootstrap policy; specialist English artifacts keep their
contracts. Its local definition is the materialization default, not a sixth
direct upstream candidate. See [`origins.md`](origins.md) for the pinned influence,
retained license, and intentionally omitted upstream assets.

Artifact schemas are contracts, not required per-role skills. Read
[`../schemas/artifacts.md`](../schemas/artifacts.md) directly. The existing
`harness-artifacts` definition is retained only as an optional adapter
translation reference; it is not assigned to every artifact-producing role.

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

Use [`../harness.yaml`](../harness.yaml) as the canonical materialization-source
registry; this catalog records the role methods, purpose, and historical origin.
Before approval, resolve each selected method to exactly one source, review that
source for compatibility, and record its repository, path, resolved full commit
SHA, access date, compatibility decision, and required/included/omitted
supporting assets in the approval packet. Mutable refs such as `main` must be
resolved at application time.

The five direct candidates (`context-engineering`, `test-driven-development`,
`security-and-hardening`, `documentation-and-adrs`, and
`doubt-driven-development`) use their upstream `SKILL.md` definitions when
compatibility review passes. Their local definitions are recorded fallbacks
only when the upstream candidate is unavailable or has a source-specific format
or compatibility issue and the host can load the local definition natively.
Lack of a native skill mechanism is not a fallback condition: obtain approval
for a translation into a supported native mechanism or explicitly omit the
skill as unsupported. Other skills follow the registry's local defaults and
optional-method mappings; aliases and same-slug candidates are not automatic
equivalents.

For the chosen `SKILL.md`, recursively inspect linked files and references.
Include required skill-local and repository-level files, including root-level
`references/` assets when referenced; per-skill install commands may omit these.
Record every asset as required, included, or omitted with an omission reason.
Harness lifecycle, approval, role, no-commit, concurrency, and artifact
contracts take precedence over generic skill methods. If the source body itself
must be changed to comply, record a `local_adaptation` and its upstream
influence. Never materialize both the upstream and local definition of one
method.

Materialize only skills needed by selected roles and capabilities. A required
or conditional role mapping determines which methods are in scope; conditional
triggers control use, not installation. The full workflow includes knowledge
management and its skills automatically. A partial scope may select knowledge
management independently; selected roles outside that capability may omit
`knowledge-base` with an explicit outside-scope reason. This is an adoption
boundary, not a per-run opt-in. For Codex, a verified native destination may be
`.agents/skills/<name>/SKILL.md`;
for Claude Code, it may be `.claude/skills/<name>/SKILL.md`. These are examples,
not defaults: use only destinations verified for the actual host. If a host has
no native skill mechanism, obtain approval for a translation into a supported
native mechanism or explicitly omit the method as unsupported; do not silently
use a local definition as a substitute. Never silently substitute a different
source or method, and do not leave prompts referring to a method that was
omitted or cannot be loaded. The role-to-skill map in
[`../AGENTS.md`](../AGENTS.md) is canonical.
