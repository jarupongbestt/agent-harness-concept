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
| [security-and-hardening](security-and-hardening/SKILL.md) | Planner, Test Engineer, Implementer, Reviewer | Planner: when a concrete affected data, permission, or action boundary and operation require security invariants, acceptance criteria, or validation planning; Test Engineer: when authoring relevant security tests for a concrete affected data, permission, or action boundary and operation whose required behavior lacks sufficient existing coverage; Implementer: when implementation affects an identified data, permission, or action boundary and operation; Reviewer: when the actual diff affects a concrete data, permission, or action boundary and operation, including concrete risk discovered during review | Local adaptation influenced by Cloudflare security-audit-skill, retaining historical template-harness and secure-development provenance |
| documentation-and-adrs | Knowledge Curator | Required for every full-workflow knowledge assessment and included partial knowledge scope | Local adaptation of knowledge-base |
| knowledge-base | Main Agent, Intake, Planner, Knowledge Curator | Required for full workflow and partial scopes including knowledge; use the configured system | Adapted from knowledge-base |
| [code-quality](code-quality/SKILL.md) | Implementer, Reviewer; conditional Planner, Test Engineer | Required for Implementer and Reviewer; Planner when choosing new or changed dependencies, shared abstractions, or state boundaries; Test Engineer when adding or changing helpers, fixtures, or nontrivial setup or assertions | Local adaptation of Ponytail and Minimize Reader Load, retaining the historical Karpathy principles |
| doubt-driven-development | Main Agent only | Conditional for non-trivial, high-stakes, unfamiliar, or hard-to-verify decisions; fresh-context adversarial challenge | Adapted from addyosmani/agent-skills; orchestration details made portable |
| [human-readable-communication](human-readable-communication/SKILL.md) | Main Agent only | Conditional for long or complex explanations, user-facing Plan presentations, and reports; preserve meaning while making the presentation easier to follow | Local adaptation of danyuchn/asd-ste100-skill v0.4.0; relaxed and language-aware |

`root-cause` is a reusable method, not a standalone agent. The same principle
applies to all skills: the role prompt owns the stage and the skill supplies a
method only when its trigger applies. `doubt-driven-development` belongs to the
Main Agent's orchestration and must not be placed in the Reviewer persona.

Select `security-and-hardening` from ordinary scope/planning information or the
actual diff identifying the concrete affected boundary and operation, including
indirect private caches, derived data, and exports. Titles and security keywords
alone neither trigger nor exclude it. Load only relevant skill/reference sections;
selection adds no mandatory per-task screening, field, checklist, scan, report,
specialist invocation, gate, or universal loading. Main Agent coordinates normally;
Verifier runs existing approved checks. The existing roles, test handoffs, and
`action: none` no-stage rule remain authoritative. A full audit needs separately
approved scope. See the [canonical map](../AGENTS.md#canonical-role-to-skill-map).

`human-readable-communication` guides the Main Agent's user-facing presentation.
Short messages use the bootstrap policy; specialist English artifacts keep their
contracts. Its local definition is the materialization default, not a
direct upstream candidate. See [`origins.md`](origins.md) for the pinned influence,
retained license, and intentionally omitted upstream assets.

`code-quality` uses one local definition classified as `local_adaptation`, with
the historical Karpathy principles retained. It guides complete, scoped changes,
suitable reuse, and readable code and tests. The conditional Planner and Test
Engineer triggers above supplement their existing methods. See
[`origins.md`](origins.md#code-quality-adaptation) for immutable influences,
adaptation decisions, and the required combined MIT license asset; no upstream
workflow or runtime is imported.

Artifact schemas are contracts, not required per-role skills. Read
[`../schemas/artifacts.md`](../schemas/artifacts.md) directly. The existing
`harness-artifacts` definition is retained only as an optional adapter
translation reference; it is not assigned to every artifact-producing role.

## Optional capabilities

| Skill | Selection and purpose | Origin |
|---|---|---|
| [pr](pr/SKILL.md) | Independently selected publication capability coordinated by Main Agent: `open` returns a PR URL; `ship` adds independent review and current checks under approved scope | Original local synthesis |

`pr` is not a required or conditional Main Agent role method. Neither selecting
Main Agent nor the full workflow installs it; `skills.definitions` lists available
sources only. Its required supporting assets are [open/reuse](pr/references/open.md)
and [ship](pr/references/ship.md). Resolve the local source's full commit SHA,
access date, compatibility, and recursively linked assets before downstream
approval; translate or reference canonical contracts and rewrite projected links.
The [canonical PR policy](../SPEC.md#optional-pr-capability) defines behavior;
[adapter guidance](../adapters/README.md#optional-pr-mapping) defines discovery and
native invocation evidence. No native command, tool backend, or runtime support
is supplied by this catalog.

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

The three direct candidates (`context-engineering`, `test-driven-development`,
and `doubt-driven-development`) use their upstream `SKILL.md` definitions when
compatibility review passes. Their local definitions are recorded fallbacks
only when the upstream candidate is unavailable or has a source-specific format
or compatibility issue and the host can load the local definition natively.
Lack of a native skill mechanism is not a fallback condition: obtain approval
for a translation into a supported native mechanism or explicitly omit the
skill as unsupported. Other skills follow the registry's local defaults and
optional-method mappings; aliases and same-slug candidates are not automatic
equivalents.

`security-and-hardening` uses one local definition classified as
`local_adaptation`; the former addyosmani same-slug candidate is excluded, with
no duplicate `security-audit` skill. Resolve the actual selected local source's
full commit SHA, access date, compatibility, and assets before application approval.
Include its required [boundary cases](security-and-hardening/references/boundary-cases.md)
and [LICENSE](security-and-hardening/LICENSE), retain the notice, and rewrite links
into approved destinations. The pinned Cloudflare influence records provenance,
not the local adaptation's revision. See
[source and asset decisions](origins.md#security-and-hardening-adaptation).

`documentation-and-adrs` uses its single local definition as a `local_adaptation`,
with historical knowledge-base attribution preserved; it is not an upstream
candidate or fallback. Applications still resolve the chosen source's full commit
SHA, access date, compatibility, and supporting assets before approval. Materialize
required ADR templates and knowledge rules into approved native destinations,
rewriting links. Map applicable final-write integrity checks to a compatible existing
target checker, translated checks, or the optional reference checker. Selecting the
reference checker also requires its Python 3.10+ standard-library runtime,
target-specific classification layout, and check command. Inventory included,
translated, referenced, and omitted assets with reasons; rewrite or remove projected
links. Illustrative ADR, log, and layout assets need not be copied. Checker maintainer
tests are not automatically created, run, or copied by workflow adoption. Required
failed or unavailable checks remain `blocked`. Do not copy the example knowledge tree.
Canonical bootstrap and role instructions retain retrieval and final-write check
rules even when a compatible upstream context-engineering skill is selected.

For the chosen `SKILL.md`, recursively inspect linked files and references.
Include required skill-local and repository-level files, including root-level
`references/` assets when referenced; per-skill install commands may omit these.
Record whether every asset is required, then its included, translated, referenced,
or omitted disposition and reason. Rewrite or remove links to assets not retained.
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
