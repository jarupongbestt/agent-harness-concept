# Changelog

## [Unreleased]

- Add optional, independently selected `pr` actions `open` and `ship`, with
  selective publication, current independent review/check evidence, bounded repair,
  and evidence-backed native mapping guidance for Codex, Claude Code, Pi, and Hermes.
  Preserve local Finalize, disabled Git-action defaults, and user-controlled merge.

## [0.3.2] - 2026-10-20

- Require completed Test Engineering and an attributed handoff before the same
  slice's Builder starts for `test_subtask.action: create` or `extend`, including
  when test and production paths are disjoint or independence from test output
  is claimed. `action: none` adds no Test Engineer stage.
- Define the handoff's slice, role, approved scope, changed test paths,
  acceptance-criteria coverage, and execution results or explicit not-run
  statement. Completed authoring may include expected red tests for missing
  behavior; passing tests are not required, while failed, blocked, or incomplete
  authoring does not release the Builder.
- Require approval, readiness, and recorded independence before overlapping
  different slices, covering scopes, read/write files, shared helpers,
  fixtures/databases, resources, mutable state, ordering, and tests observing
  unfinished changes in the shared `in_place_uncommitted` worktree. Separate
  task IDs, outputs, or disjoint writes do not prove independence; serialize
  uncertain or conflicting invocations with a recorded reason.
- Align Plan, conflict-check, role, and Task Result contracts with these rules.
  This portable workflow clarification requires neither mandatory worktrees nor
  an executable scheduler and preserves approval, later-stage serialization,
  bounded retries, stage re-entry rules, and disabled Git-action defaults.

## [0.3.1] - 2026-10-15

- Distinguish authoritative project documentation, evidence-backed reusable
  knowledge, and execution-specific run artifacts by purpose and authority.
- Keep one authoritative home per substantive record; cite canonical documents
  and decisions instead of copying them into knowledge. Preserve distinct supported
  learning, related topic overlap, and unresolved contradictions.
- Bound Curator permissions: curation cannot silently establish or revise
  requirements or grant general project-documentation write access. Ordinary
  project documents do not inherit knowledge metadata from their topic or location.
- Limit application mapping to the selected scope and reuse approved existing
  homes only when purpose and ownership fit, without guessed destinations or
  mandatory layouts.
- Align bootstrap, role instructions, artifact guidance, and documentation methods
  while preserving lifecycle, knowledge integrity, and ADR safeguards.

## [0.3.0] - 2026-10-15

- Route messages by intent before Intake: answer ordinary conversation directly,
  preserve active execution during follow-up questions, and interpret affirmatives
  in context while retaining complete Plan approval before project edits.
- Preserve the same run and its artifacts for continuing objectives, with recorded
  evidence and permitted triggers before stage re-entry; renew approval for
  material Plan changes.
- Add a language-aware, meaning-preserving Main Agent communication policy and
  `human-readable-communication` skill, with recorded upstream provenance and
  the retained MIT license.
- Include knowledge management in the full workflow automatically, with configured
  navigation and skill loading for Main Agent, Intake, and Planner, plus a
  post-review Curator assessment and retention of eligible durable learning.
- Require a reasoned Knowledge Outcome for finalization: completed updates,
  assessed no-change, blocked retention with remaining work, or an explicit
  outside-scope result for partial adoption without knowledge. Preserve independent
  knowledge-only adoption and approved target-native paths and protection rules.
- Define ordered knowledge retrieval through startup navigation, domain Navigation
  Hints, relevant topics, and focused `source_refs`; require recorded reasons for
  bounded expansion when navigation is absent, stale, contradictory, or insufficient.
- Strengthen topic metadata, typed references, dates, status transitions, category
  protection, action logs, and checks after the final knowledge write. Add an ADR
  template with preserved history and `Proposed`, `Accepted`, `Superseded`, and
  `Deprecated` states; acceptance requires retained human decision evidence.
- Provide an optional read-only knowledge validator with explicit classification
  layout and maintainer regression tests. It uses Python 3.10+ standard library
  only and a documented bounded YAML subset, with no third-party packages or
  installation step. Its mechanical checks do not prove claims or authenticate
  human decisions.
- Use the single local `documentation-and-adrs` definition as a `local_adaptation`
  with historical knowledge-base attribution; retain the four direct upstream
  candidates and their compatibility-based source selection.
- Clarify downstream materialization of required ADR and knowledge assets, native
  destinations, rewritten links, and compatible final-write checks. Only selecting
  the optional checker requires its runtime and target-specific layout; adoption
  does not copy the whole example tree or automatically create, run, or copy the
  checker's maintainer tests.

## [0.2.0] - 2026-09-27

- Make the lifecycle plan-first: Intake completes, Planner produces the full
  Plan, and the user approves it before implementation. Ordinary implementation,
  test, lint, build, or verification failures retry the affected Builder slice
  within a bounded budget and do not loop back to Intake or Planner.
- Restrict overlapping specialist work to dependency-ready, non-conflicting
  Test Engineer and Builder invocations. Verifier, Reviewer, Knowledge Curator,
  and other specialists run after active test/build work and without specialist
  overlap.
- Make knowledge curation and durable knowledge writes opt-in. The example
  knowledge tree is not a default target-project path.
- Clarify partial adoption and native mapping: adopters can select capabilities
  or roles, discover host-native destinations, and materialize only the selected
  scope rather than copying the example repository layout.
- Rename Test Designer to Test Engineer and publish a canonical role-to-skill
  map, including the required Karpathy Guidelines skill and orchestration-only,
  conditional doubt-driven development skill.
- Define per-application skill source selection and provenance: compatibility
  review, resolved upstream commit SHA and access date, recursive supporting
  asset inspection, one definition per method, recorded local fallbacks, and
  approved translation or explicit omission when native skill support is absent.
