# Platform Adapter Requirements

An adapter makes the portable harness executable on a specific agent platform.
It is target-specific configuration and mapping only: it translates the
portable responsibilities into the target repository's native conventions. It
is not a copy, fork, installation, or reproduction of this repository.

The adapter is a mapping, not a copy of this repository's folder tree. The portable
concept does not require `.claude/agents/`, `.codex/agents/`, `.agents/skills/`, or
any other exact path. See [`../APPLY.md`](../APPLY.md) for an illustrative mapping
and the questions an applying agent must ask first.

The target may use zero, one, or multiple native configuration files, depending
on the actual platform capabilities and the target repository's existing setup.
Do not create adapter files merely to match the examples below; create or
modify only the approved target-specific files.

## Native Convention Discovery

Native Convention Discovery is an adapter responsibility and a mandatory,
read-only prerequisite for every adapter/application mapping. Complete it
before concept mapping, proposing a target file map, or any write. The adapter
must inspect the actual host and target repository and report the detected
platform, platform version or release/channel, operating mode, host runtime and
version, and host capabilities relevant to the mapping. It must also inspect
the target's existing instructions, configuration, agent/session mechanisms,
skills, checks, and applicable project constraints. Discover existing
project-documentation, knowledge, and run-artifact homes only when relevant to
the requested adoption scope.

The discovery report must use current, mutually consistent first-party
evidence. For each material finding, record a first-party source URL (or exact
first-party source path where a URL is not applicable), access date, applicable
platform version and operating mode, finding, confidence, and
`verification_evidence_ref`. For every portable capability that the mapping will
`reuse` or `translate`, record the exact native destination and the mechanism by
which the host loads, invokes, or enforces it, but only when that destination and
mechanism are verified.

Use these canonical fields for each auditable finding and keep them consistent
with `schemas/artifacts.md`:

```yaml
capability: specialist_delegation
status: verified                 # verified | unknown | blocked
source_url: https://first-party.example/docs/native-conventions
source_path: null                # exactly one of source_url/source_path
accessed_date: YYYY-MM-DD
applicable_platform_version: "release-or-channel"
applicable_operating_mode: local
finding: "The host supports isolated specialist sessions"
confidence: high
exact_native_destination: "exact verified native destination"
mechanism: "exact load, invoke, or enforcement mechanism"
verification_evidence_ref: "source-1 or exact verification command/record"
```

Exactly one of `source_url` or `source_path` is required; `source_path` must be
an exact host or first-party path. `applicable_platform_version`,
`applicable_operating_mode`, `finding`, and `confidence` are required. Every
destination has its own `status`, and discovery is `verified` only when every
relevant destination and evidence record is verified.

Unknown, unsupported, contradictory, stale, or low-confidence conventions must
not be guessed or promoted into a file map. Mark the affected capability
`unknown` or `blocked`, stop before mapping, approval to proceed, or writing,
and ask the user for the missing information or report the evidence gap. In
particular, a path that exists and an example from this repository are not
evidence that a host supports a native convention. A discovery, concept map, or
file map with `unknown` or `blocked` status forbids `approval.decision:
proceed` and target writes.

The examples `.claude/`, `.codex/`, `.agents/`, `CLAUDE.md`, and `AGENTS.md` are
illustrative and non-normative only; they are never evidence, defaults, or
universal destinations. The same applies to every path in the illustrative
mapping below. An example may be used only after independent discovery verifies
the actual host's destination and mechanism.

The application output must be a native projection. If a proposed target tree
looks like a copy of this repository—for example, it adds root `prompts/`,
`schemas/`, `knowledge/`, or `skills/` solely because those directories exist
here—stop before approval and return a failed source-tree-copy audit. Translate
role prompts into the host's native agent or skill format, or reference them as
source material without copying them.

Each adapter must define how to perform these capabilities. **Delegation is
required by the portable design**: the adapter must map the specialist roles to
native subagents, agent sessions, or equivalent isolated execution contexts.

| Portable capability | Adapter responsibility |
|---|---|
| Main Agent bootstrap | Map the bootstrap contract into the target platform's native startup mechanism. [`../AGENTS.md`](../AGENTS.md) is source material only; it is never a target path to copy or load literally. |
| Read files | Map to the platform's file-reading mechanism |
| Write files | Map to scoped editing tools |
| Delegate | Spawn or invoke isolated role subagents |
| Ask user | Implement the approval gate |
| Run checks | Invoke shell, test, or validation tools |
| Permissions | Restrict tools and write paths by role |
| Structured output | Preserve the artifact contracts |
| Knowledge update | Allow only the Knowledge Curator write path |

For the selected scope, map relevant record homes according to the
[record-boundary policy](../SPEC.md#documentation-knowledge-and-run-artifacts).
Use existing inventory, mapping, and scope descriptions to record their purpose,
authority, owner, and approved write rules. Preserve suitable discovered/approved
homes and ownership; a general documentation area is not an implicit learning or
run home. Never guess unresolved destinations or create missing ones without
applicable approval. Curation grants no general project-documentation write
permission or authority to silently revise requirements.

## Skill source resolution

Before approval, resolve the source for every selected skill using the registry
and policy in [`../harness.yaml`](../harness.yaml), then review the exact source
body for compatibility with the harness. For the three direct candidates
(`context-engineering`, `test-driven-development`, and
`doubt-driven-development`), prefer the upstream
definition when that review passes. Record the repository, path, resolved full
commit SHA, access date, compatibility decision, and supporting assets in the
approval packet. The mutable `main` branch is only a lookup ref; resolve its
commit at application time. A local definition is a fallback only when the
upstream candidate is unavailable or has an upstream-specific source, format,
or compatibility problem and the host can load the local skill natively; record
the fallback and reason. A host-wide lack of native skill support never permits
automatic local fallback. Obtain approval for translation into a supported
native mechanism or explicitly omit the skill with a reason.

`security-and-hardening` uses the single local `local_adaptation`; the former
addyosmani same-slug candidate is excluded, not a fallback or duplicate method.
Resolve the actual selected local source's full commit SHA, access date,
compatibility, and assets before application approval. Include its required
`references/boundary-cases.md` and `LICENSE`, retain the notice, and rewrite links
into approved destinations. The pinned Cloudflare influence records provenance,
not the revision of the local adaptation. See
[source and asset decisions](../skills/origins.md#security-and-hardening-adaptation).
Preserve the [conditional role triggers](../AGENTS.md#canonical-role-to-skill-map):
ordinary scope/planning information or the actual diff identifies a concrete
affected data, permission, or action boundary and operation, including indirect
private caches, derived data, and exports. Titles and security keywords alone
neither trigger nor exclude it. Load only relevant skill/reference sections;
add no mandatory per-task screening, field, checklist, scan, report, specialist
invocation, gate, or universal loading. Main Agent coordinates normally and
Verifier runs existing approved checks; existing roles, lifecycle, and test
handoffs apply. A full audit needs separately approved scope.

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

For all other skills, follow their local defaults and optional method mappings
in the registry. Slug similarity alone does not establish equivalence. Inspect
the chosen `SKILL.md` recursively for linked files and references; include
required skill-local and repository-level assets such as root-level
`references/` files, which per-skill install commands can omit. Record every
required asset's included, translated, referenced, or omitted disposition and reason.
Materialize exactly one definition per
selected method and only for selected roles/capabilities. Harness lifecycle,
approval, role, no-commit, concurrency, and artifact contracts take precedence.
If the source body needs edits to comply, classify it as a local adaptation and
record its upstream influence. Unsupported native loading requires an approved
translation into a supported native mechanism or an explicit omission; never
silently substitute a source or method.

## Optional PR mapping

Select `pr` independently; neither full-workflow nor Main Agent selection implies
it. The [canonical policy](../SPEC.md#optional-pr-capability) and
[skill](../skills/pr/SKILL.md) own the procedure. Map one skill with `open`/`ship`
action text, not a new role, lifecycle stage, built-in command, or mandatory CLI.
Include/translate its required `references/open.md` and `references/ship.md`,
inspect recursive links, and rewrite or reference canonical contracts under the
existing source-resolution rules.

These first-party research anchors were accessed **2026-11-25**. The candidate
entrypoints project native skill invocation after registration; they are not
installed or tested commands. Verify the actual version, operating mode, runtime,
destinations, loading, discovery, names/collisions, and action argument delivery.
Documentation evidence is neither target runtime discovery nor post-write proof.

| Runtime | Candidate entrypoints and evidence boundary |
|---|---|
| Codex | `$pr open` / `$pr ship` project CLI/IDE skill mentions; `/skills` provides selection. [Skills documentation](https://learn.chatgpt.com/docs/build-skills) describes repository `.agents/skills/` discovery and optional `agents/openai.yaml` policy `allow_implicit_invocation: false`. Verify the actual client; do not assume identical syntax on every app surface. |
| Claude Code | `/pr open` / `/pr ship` apply only when that skill name is registered. [Skills documentation](https://code.claude.com/docs/en/skills) describes trailing arguments through `$ARGUMENTS` or appended text, source-dependent namespacing/collisions, and `disable-model-invocation`. Managed permission rules can override skill `allowed-tools`; verify effective permissions. |
| Pi | `/skill:pr open` / `/skill:pr ship` project [skill commands](https://raw.githubusercontent.com/earendil-works/pi/main/packages/coding-agent/docs/skills.md); trailing text becomes the user request. `enableSkillCommands` controls interactive discovery; manually entered commands still work. The [coding-agent README](https://raw.githubusercontent.com/earendil-works/pi/main/packages/coding-agent/README.md) says subagents and plan mode are not bundled; discover an approved extension or equivalent isolated execution instead of assuming delegation. |
| Hermes | `/pr open` / `/pr ship` project [installed-skill commands with task text](https://hermes-agent.nousresearch.com/docs/user-guide/cli). [Command documentation](https://hermes-agent.nousresearch.com/docs/reference/slash-commands) gives built-ins collision priority and `/skill <name>` as a loading fallback, subject to messaging privileges. [Skills documentation](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) describes the primary `~/.hermes/skills/` location and configured external directories. Verify the actual CLI/TUI/gateway surface, discovery, fallback, and action argument delivery. |

Native invocation controls may be proposed only in the approved target mapping;
this portable source adds no runtime configuration. Implicit discovery or loading
never grants publishing authority, even where automatic matching remains enabled.
Skill support alone proves neither delegation, permission enforcement, sandboxing,
nor checkpoint/resume support. Preserve OpenCode and other hosts through the same
evidence-based discovery gate, without inferring their invocation from this table.

For `open` or `ship`, discover Git state/diff, exact host/repository/fork PR lookup,
and selective state preservation and partial-effect reconciliation. Verify the
actual CLI, API, or connector and effective permissions for selected commit,
push, create, and update actions. Command examples and general documentation
alone leave tool availability and access unverified.

For `ship`, also verify access to host rules, check/job state, actual tested
revisions, and relevant logs, plus bounded Main Agent watch/poll and isolated
independent full-diff review. Discover rerun permissions or repair dependencies
when those capabilities are selected, and verify their authorization before use.
For observation and resume, discover per-call and live-session wait limits,
permitted cadence, and what ends the invocation or local session. Record the
operation's repository timing evidence or explicit queue-plus-CI expectation,
cadence, finite count, and deadline separately from runtime limits. Map an approved
checkpoint mechanism and purpose-compatible record home when retention is needed.
Every continuation reconciles current authority, exact PR and head/base, current
checks and actual tested revision, completed/partial effects, and cumulative
counters. Use the existing
[PR Operation Result](../schemas/artifacts.md#pr-operation-result), not a new
monitor artifact or capability registry.

Bounded foreground observation and manual resume are the portable `ship` baseline
for Codex, Claude Code, Pi, Hermes, OpenCode, and other hosts. For a selected live
scheduler, verify that the configured, authorized native mechanism delivers
bounded status-only observations in a still-live session. `next_observation_at`
records a planned time and configures no scheduler. When post-exit continuation
is selected, verify actual delivery and resume through an existing authorized
receiver and runtime with access to the same approved durable checkpoint. A
repository event can supply a signal. CI alone supplies no dispatch. Treat
payloads as untrusted signals.
Before enabling competing triggers, verify target-native single continuation per
operation across scheduled, event-triggered, and manual paths. When delivery or
serialization cannot be verified, use bounded foreground observation and serial
manual resume.

Follow the [application protocol](../APPLY.md#application-protocol) before
selecting a wake-up mechanism. Full adoption retains applicable core stages,
roles, contracts, approval, knowledge management, local Finalize, and disabled
Git-action defaults. Selected partial adoption includes requested items and their
explicit dependencies, with omitted items and their reasons recorded.

`open` can stand alone with its declared capabilities and returns the PR URL and
stops. `ship` repair requires an existing approved lifecycle/roles/scope and retry
provenance; selecting it does not install or authorize a missing full workflow.
Unknown required capabilities remain `blocked` under discovery rules until
resolved; unsupported mechanisms need an approved translation or explicit
omission, never silent single-agent collapse.

For **either `open` or `ship`**, verify these shared behaviors.

- registration, action/argument delivery, collisions, and implicit loading without
  mutation authority, plus effective tool permissions for selected actions;
- exact repository/fork PR reuse, selected/unrelated work preservation, and
  same-operation recovery after partial commit, push, or PR creation, including
  an invocation ending after a partial push and resuming without duplicate
  publication or reset counters.

For **`open`**, verify that it returns the exact PR URL and stops.

For **`ship`**, verify review, checks, and bounded observation.

- current independent review of the entire PR diff and required gates for the
  same head/base under actual host rules, with actual tested revision and
  coverage evidence;
- queued/pending checks progressing to pass through bounded foreground
  observation; an invocation ending while waiting with remaining allowance
  returns `pending`, exhausted count/deadline returns `blocked` without resetting
  budgets, and only explicit stop returns `stopped`;
- unavailable required material logs or tested-revision evidence returning
  `blocked`; cancelled, timed-out, action-required, unknown, and absent checks
  classified under the
  [ship procedure](../skills/pr/references/ship.md#observe-within-remaining-allowance),
  with raw check state, gate acceptance, actual coverage, and operation status
  reported separately rather than assumed passed;
- head/base movement invalidating affected evidence; missing CI without setup
  authority yielding authorized local evidence and explicit remote limitations,
  with no unapproved CI setup or claim that a required remote gate passed;
- effective check permissions, checkpoint and record home, observed cadence and
  per-call/live-session limits, distinct cumulative operation budgets, and
  resume reconciliation without resetting usage/history on budget changes.

For **selected and authorized `ship` capabilities**, verify the applicable case.

- **Repair** hands off a supported failure with relevant logs, exact
  PR/head/base/tested revision, approved slice/write authority, and cumulative
  retry history through existing evidence/result references. Follow existing role
  ordering, publish verified work to the same PR, and refresh independent
  full-diff review and checks for the resulting revision.
- **Infrastructure reruns** use current classified evidence, effective
  permissions, and the separate bounded cumulative rerun budget.
- **Wake-up** requires observed authorized delivery for each selected mode. If
  live scheduling is selected, verify bounded status-only observation. If post-exit
  delivery is selected, verify resume from the same approved shared durable
  checkpoint. For competing triggers, verify single continuation across scheduled,
  event, and manual paths. Stale, duplicate, delayed, or overlapping triggers
  preserve budgets and leave stopped operations and closed/merged PRs alone.
  They cannot duplicate repair or publication or cause action on stale revisions.

For `ship`, verify the foreground and serial-manual fallback when optional wake-up
is unavailable. Check the selected scope and dependencies for every mapping.
Full adoption keeps all applicable core workflow and knowledge requirements.
Partial adoption records omissions and needs verified support for every selected
mechanism.

Use safe native diagnostics or approved fixtures; mutation exercises require their
own authorized target scope. Record unavailable evidence explicitly, and do not
claim these runtimes have been tested by this portable documentation change.

Suggested adapter files (illustrative and non-normative; do not create these by
default):

```text
adapters/<platform>/
├── README.md
├── bootstrap.md
├── role-mapping.md
├── permissions.md
└── install-or-run.md
```

Platform path examples (illustrative and non-normative; verify before use):

```text
Claude   → CLAUDE.md, .claude/agents/, and .claude/skills/
Codex    → AGENTS.md, .codex/agents/, and .agents/skills/
Pi       → discovered instruction/skill locations and approved delegation extension
OpenCode → .opencode/ agents, tools, and plugins
Hermes   → Hermes skills and tool configuration
```

For the current evidence behind these examples, consult the official
[Codex instruction guidance](https://learn.chatgpt.com/docs/agent-configuration/agents-md),
[Codex subagent guidance](https://learn.chatgpt.com/docs/agent-configuration/subagents),
and [Codex skills guidance](https://learn.chatgpt.com/docs/build-skills), plus
Claude Code's [instruction guidance](https://code.claude.com/docs/en/memory),
[skills guidance](https://code.claude.com/docs/en/skills), and
[subagent guidance](https://code.claude.com/docs/en/agents). These links do not
replace per-run discovery and native post-write verification.

## Native post-write verification

After an approved mapping is written, the adapter/application must verify
natively that each written destination and mechanism is recognized, loaded,
invoked, or enforced by the host. File existence alone is insufficient. Use the
host's native diagnostic, effective-configuration view, registration or API
result, session/delegation check, or equivalent mechanism, as applicable.

Record exact verification evidence for every destination using a per-destination
record with `status: verified`, `unknown`, or `blocked`; `exact_native_destination`,
`mechanism`, `verification_evidence_ref`, source URL or exact source path,
`accessed_date`, `applicable_platform_version`,
`applicable_operating_mode`, `finding`, and `confidence`. The overall result is
`verified` only when every written destination has a verified record and complete
evidence. If native verification cannot be completed or evidence fails, mark the
affected record `unknown` or `blocked`, stop before additional target writes, and
do not claim success.

The adapter must not redefine the workflow or introduce a different meaning for
Ticket, Plan, approval, verification, or knowledge provenance. It must not imply
that the Main Agent performs all specialist work itself when the host supports
subagents. The adapter is execution mapping only: it must not redefine the
portable lifecycle or imply one fixed directory or file layout.
