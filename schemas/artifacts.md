# Portable Artifact Contracts

These are logical contracts. Adapters may represent them as JSON, YAML, Markdown,
or native tool results, but the fields and meaning should remain compatible.

Apply the [record-boundary policy](../SPEC.md#documentation-knowledge-and-run-artifacts)
when describing project documentation, durable knowledge, and run artifacts.
Record scope-relevant purpose, canonical home, owner, approved destination and
write limits in existing inventory, mapping, scope, evidence, and result/outcome
records or adapter-native equivalents. This adds no mandatory artifact fields or
ADR inventory. Classification follows purpose and authority, not folder names;
ordinary project documents do not inherit knowledge metadata from topic or location.

## Evidence Record

Evidence records use the same fields in discovery, concept/file mapping, and
native post-write verification. Exactly one of `source_url` or `source_path`
must identify the source; `source_path` is an exact host or first-party source
path, not an inferred or illustrative path. `verification_evidence_ref` points
to the exact command, native diagnostic, effective-configuration view,
registration/API result, session/delegation check, or evidence record used for
verification.

```yaml
evidence_id: evidence-123
status: verified              # verified | unknown | blocked
source_url: https://first-party.example/docs/native-conventions
source_path: null             # use instead of source_url when a URL is not applicable
accessed_date: YYYY-MM-DD
applicable_platform_version: "release-or-channel"
applicable_operating_mode: local
finding: "The host recognizes the native mechanism"
confidence: high              # high | medium | low
exact_native_destination: "exact verified destination or null"
mechanism: "exact load, invoke, enforce, or verification mechanism"
verification_evidence_ref: "exact command, source, or evidence reference"
```

`source_url` and `source_path` are mutually exclusive and one is required.
`applicable_platform_version`, `applicable_operating_mode`, `finding`, and
`confidence` are required for every material finding. Low-confidence, stale, or
contradictory evidence has `status: unknown` or `status: blocked` and cannot
authorize a mapping or write.

## Native Discovery

Native Convention Discovery is a read-only prerequisite for concept mapping,
file-map proposals, and writes. It records the actual host and target
inventory, rather than prescribing a platform layout. A discovery has status
`verified` only when the relevant evidence and destinations are verified;
`unknown` records an unresolved convention, and `blocked` records a condition
that requires stopping before a write.

```yaml
discovery_id: discovery-123
run_id: run-123
status: unknown              # verified | unknown | blocked
platform: codex
platform_version: "release-or-channel"
operating_mode: local        # local | hosted | cloud | plugin | integration | other
host_runtime:
  name: runtime-name
  version: "runtime-version"
target_inventory:
  repository_root: /path/to/target
  instruction_files: []
  native_configuration: []
  agent_subagent_mechanisms: []
  skills: []
  checks: []
  knowledge_paths: []       # relevant configured/approved knowledge homes
  run_state_paths: []       # relevant configured/approved execution-record homes
  constraints: []           # include scope-relevant project-document homes, owners, and write limits
target_layout:
  status: unknown              # verified | unknown | blocked
  native_projection_preview: []
  source_tree_reproduction: unknown  # true | false | unknown
  unapproved_source_tree_paths: []
  verification_evidence_ref: null
host_capabilities:
  - capability: delegate
    status: verified       # verified | unknown | blocked
    finding: "Host can invoke isolated specialist sessions"
    evidence_refs: [source-1]
first_party_sources:
  - source_id: source-1
    status: verified       # verified | unknown | blocked
    source_url: https://first-party.example/docs/native-conventions
    source_path: null      # exact first-party path when a URL is not applicable
    accessed_date: YYYY-MM-DD
    applicable_platform_version: "release-or-channel"
    applicable_operating_mode: local
    finding: "The host loads the verified native mechanism"
    confidence: high       # high | medium | low
    verification_evidence_ref: source-1
capability_destinations:
  - capability: main_agent_bootstrap
    status: verified       # verified | unknown | blocked
    exact_native_destination: "exact verified destination"
    mechanism: "exact load, invoke, or enforcement mechanism"
    verification_evidence_ref: source-1
    evidence_refs: [source-1]
  - capability: write_files
    status: unknown
    exact_native_destination: null
    mechanism: null
    verification_evidence_ref: null
    evidence_refs: []
    unknown_blocker_reason: "The host convention could not be established"
unknown_blocker_reasons:
  - capability: write_files
    status: unknown       # unknown | blocked
    reason: "Missing or contradictory first-party evidence"
    required_action: "Ask for the missing information or report the run blocked"
```

Each `first_party_sources` record must contain `status`, exactly one of
`source_url` or `source_path`, `accessed_date`,
`applicable_platform_version`, `applicable_operating_mode`, `finding`, and
`confidence`, and `verification_evidence_ref`. Each material finding must
reference current, mutually consistent first-party evidence. Each capability
that is reused or translated must have a verified `exact_native_destination`,
`mechanism`, and
`verification_evidence_ref`. Every destination has its own `status`; discovery
is `verified` only when all relevant destinations and their evidence are
verified. Unknown, unsupported, stale, contradictory, or low-confidence
evidence must be `unknown` or `blocked` and must not be promoted into a file
map. Unknown destinations cannot be guessed. Illustrative paths such as
`.claude/`, `.codex/`, `.agents/`, `CLAUDE.md`, and `AGENTS.md` are not evidence
of a native convention. Native verification is required after an approved write;
file existence alone is insufficient, and the result belongs in the Run Summary.
This contract does not create platform config files or hardcode a universal
native layout.

`target_layout` is required for an application that writes files. The preview
must show the target-native destinations before approval. `source_tree_reproduction`
must be `false`, and `unapproved_source_tree_paths` must be empty, before the
application may proceed. A value of `true` or `unknown` blocks approval and target
writes; an existing target path may be retained only when the inventory and map
give it an independent reason.

Use the existing inventory fields and evidence findings to record the selected
scope's relevant project-documentation, knowledge, and run homes and ownership.
Reuse a home only when its discovered or approved purpose and ownership fit the
record; a general documentation area is not an implicit learning or run home.
Carry the supported placement and permission decisions into the applicable
Concept Map and Target File Map entries. Unresolved destinations remain explicit
gaps, never invented paths.

## Concept Map

The Concept Map preserves the portable concept while recording how the actual
host supports it. Every covered concept has exactly one action: `reuse`,
`translate`, `reference`, or `omit`. `reuse` and `translate` require a verified
exact native destination, mechanism, and linked discovery evidence.

```yaml
concept_map_id: concept-map-123
discovery_id: discovery-123
status: unknown               # verified | unknown | blocked; an unknown entry is illustrative
skill_materializations:
  - skill: context-engineering
    selection_status: materialized # materialized | reused_existing | omitted | unsupported
    materialization_source:
      kind: local_definition      # upstream_direct | local_definition | local_adaptation | target_existing_definition
      repository: https://github.com/jarupongbestt/agent-harness
      path: skills/context-engineering/SKILL.md
      ref: HEAD                   # mutable name only; record the resolved SHA below
      resolved_commit_sha: "<full 40-character SHA resolved at application time>"
      accessed_date: YYYY-MM-DD
      adapted_from: null          # required for local_adaptation; records upstream influence, not a second materialized source
    compatibility:
      status: compatible           # compatible | adapted | incompatible | not_applicable
      precedence_contracts: [lifecycle, approval, roles, no_commit, concurrency, artifact_contracts]
      decisions: []
    supporting_assets:
      required: []                # records use {path, purpose}
      included: []                # records use {path, destination}
      omitted: []                 # each omission requires {path, reason}
    fallback:
      used: false
      from_source: null
      to_source: null
      reason: null
    omission:
      omitted: false
      reason: null
entries:
  - concept: workflow_lifecycle
    action: translate          # reuse | translate | reference | omit
    status: verified           # per-destination status
    source_material_refs: [portable-source-1]
    exact_native_destination: "target-native bootstrap or session mechanism"
    mechanism: "host mechanism that loads or invokes the concept"
    discovery_evidence_refs: [source-1]
    verification_evidence_ref: source-1
    omission_reason: null
  - concept: unsupported_capability
    action: omit
    status: unknown
    source_material_refs: [portable-source-2]
    exact_native_destination: null
    mechanism: null
    discovery_evidence_refs: [source-2]
    verification_evidence_ref: null
    omission_reason: "The host does not expose the required capability"
  - concept: source_workflow
    action: reference
    status: verified
    source_material_refs: [portable-source-1]
    exact_native_destination: null
    mechanism: null
    discovery_evidence_refs: []
    verification_evidence_ref: null
    omission_reason: null
```

Each application artifact records one `skill_materializations` item for each
selected or explicitly omitted skill. A `materialized` or `reused_existing`
skill has exactly one `materialization_source`; never materialize both a local
definition and an upstream candidate for the same skill. Resolve and record the
source's full commit SHA and `accessed_date` at application time. Candidate
branches such as `main` are mutable references, not locks; do not carry a
research-time SHA forward as the application's source pin. If an upstream body
must be changed to meet harness compatibility rules, use `kind: local_adaptation`
and record the upstream influence under `adapted_from` rather than calling it a
direct upstream materialization.

`supporting_assets.required` lists non-skill files needed by the selected source;
`included` records those actually materialized, and each `omitted` asset needs
an exact path and reason. `fallback.used` is true only when the preferred source
was unavailable or incompatible and a different single source was selected;
record both sources and the reason. If no skill source is materialized, use an
applicable `selection_status`, set `materialization_source: null`, and set
`omission.omitted: true` with a reason. Never report a silent fallback or
omission.

An `omit` entry must include `omission_reason`. A `reference` entry points to
source material without creating a target destination; `source_material_refs`
are not native evidence refs and do not establish host support. A concept map with
`unknown` or `blocked` status cannot authorize approval to proceed or any target
write.

## Target File Map

The Target File Map is the approved, target-specific projection of the Concept
Map. It is created only after verified discovery and explicit user approval.

```yaml
file_map_id: file-map-123
concept_map_id: concept-map-123
status: unknown               # verified | unknown | blocked; an unknown entry is illustrative
layout_audit:
  status: unknown              # verified | unknown | blocked
  native_projection_preview: []
  source_tree_reproduction: unknown
  unapproved_source_tree_paths: []
  verification_evidence_ref: null
entries:
  - file_map_entry_id: file-1
    concept: workflow_lifecycle
    action: translate          # reuse | translate | reference | omit
    status: verified           # per-destination status
    source_material_refs: [portable-source-1]
    exact_native_destination: "exact target file, setting, API, or session"
    mechanism: "exact native load, invoke, or enforcement mechanism"
    discovery_evidence_refs: [source-1]
    verification_evidence_ref: source-1
    omission_reason: null
  - file_map_entry_id: file-2
    concept: unsupported_capability
    action: omit
    status: unknown
    source_material_refs: [portable-source-2]
    exact_native_destination: null
    mechanism: null
    discovery_evidence_refs: [source-2]
    verification_evidence_ref: null
    omission_reason: "No verified native destination exists"
```

Every `reuse` or `translate` entry must identify the exact destination and
mechanism and link to discovery evidence. Every `reference` or `omit` entry
must not invent a destination; every `omit` entry must state its reason. Each
entry has a per-destination `status`, using only `verified`, `unknown`, or
`blocked`.

`layout_audit.status` may be `verified` only when the preview is target-native,
`source_tree_reproduction` is `false`, and no unapproved source-tree path is
listed. A missing, failed, or unknown layout audit forbids approval with
`decision: proceed` and forbids target writes.

Unknown or blocked discovery, mapping, or file-map entries forbid approval with
`decision: proceed` and forbid target writes. Stale, contradictory, or
low-confidence evidence is not a valid basis for a verified entry.

## Ticket

```yaml
ticket_id: task-123
run_id: run-123
request: "Add Google OAuth login"
change_type: feature        # feature | bugfix | refactor | cosmetic | research | documentation
scope_hints:
  - knowledge/auth/index.md
target_refs:
  - src/auth/
acceptance_criteria:
  - "Users can sign in with Google"
  - "Invalid callback state is rejected"
tier: 2                     # 0 | 1 | 2
confidence: medium          # high | medium | low
clarification_needed: false
clarification_questions: []
assumptions: []
```

## Plan

```yaml
plan_id: plan-123
ticket_id: task-123
summary: "Add server-side OAuth state handling and callback validation"
execution:
  scheduler: dependency_aware
  parallel_independent_slices: true
  lifecycle:
    intake_complete_before_planner: true
    full_plan_complete_before_approval: true
    full_plan_complete_before_implementation: true
  parallel_specialist_roles: [test_engineer, implementer]
  specialist_overlap_constraints:
    only_listed_roles_may_overlap_active_specialist_work: true
    dependencies_must_be_ready: true
    scopes_files_resources_mutable_state_and_ordering_must_not_conflict: true
    # create/extend require completed same-slice authoring and delivered handoff;
    # none adds no Test Engineer stage. Expected red tests may be handed off.
    sequence_builder_after_completed_same_slice_test_subtask: true
    cross_slice_independence_must_be_recorded: true
    shared_worktree_read_visibility_must_be_considered: true
    unknown_independence_route: serialize_affected_invocations
    verifier_reviewer_knowledge_curator_and_other_specialists_overlap_active_work: false
    all_test_engineer_and_implementer_work_completes_before_verification: true
    verification_completes_before_review: true
  default_mode: in_place_uncommitted
  retry_policy:
    max_retries_per_slice: 2
    retry_on:
      - verification_test_failure
      - acceptance_failure_with_valid_scope
      - verification_lint_failure
      - verification_build_failure
      - verification_check_failure
      - verifier_failure
    replan_only_on:
      - invalid_plan
      - scope_change
      - acceptance_criteria_change
      - dependency_invalidation
      - permission_invalidation
      - platform_invalidation
    ordinary_failure_route: retry_affected_implementer_slice
    ordinary_failures_restart_intake_or_planner: false
    exhausted_retry_route: escalate
    replan_requires_explicit_feedback_or_objective_plan_invalidation: true
    material_replan_requires_renewed_approval: true
  reentry_policy:
    intake:
      default_passes: 1
      allowed_triggers:
        - user_clarification_changes_ticket
        - newly_discovered_ticket_mismatch
    planner:
      default_passes: 1
      full_plan_before_approval: true
      full_plan_before_implementation: true
      allowed_triggers:
        - explicit_human_plan_feedback
        - invalid_plan
        - scope_change
        - acceptance_criteria_change
        - dependency_invalidation
        - permission_invalidation
        - platform_invalidation
tasks:
  - task_id: slice-1
    description: "Store OAuth state server-side"
    files:
      - src/auth/state.ts
    level: hard              # easy | hard
    depends_on: []            # readiness requires every listed dependency to complete
    parallel_eligible: true   # cross-slice only, after readiness and recorded independence
    conflict_checks:
      files:
        - src/auth/state.ts
        - tests/auth/state.test.ts # include test files and files tests/checks observe
      read_files: [src/auth/state.ts, tests/auth/state.test.ts]
      write_files: [src/auth/state.ts, tests/auth/state.test.ts]
      shared_helpers: []
      fixtures_or_databases: []
      resources: []           # shared mutable resources require serialization
      state: []               # conflicting mutable state requires serialization
      ordering: []            # explicit ordering constraints require serialization
      independence_evidence: [] # no concurrent pair is proven by this example
      serialization_reasons:
        - "Same-slice Builder waits for completed create authoring and delivered handoff"
    acceptance:
      - "State expires after five minutes"
      - "State is single-use"
    test_subtask:
      action: create          # create | extend | none
      file: tests/auth/state.test.ts
      cases:
        - "expired state is rejected"
        - "state cannot be reused"
  - task_id: slice-2
    description: "Validate state in the callback"
    files:
      - src/auth/callback.ts
    level: easy
    depends_on: [slice-1]
    parallel_eligible: true
    conflict_checks:
      files: [src/auth/state.ts, src/auth/callback.ts, tests/auth/callback.test.ts]
      read_files: [src/auth/state.ts, src/auth/callback.ts, tests/auth/callback.test.ts]
      write_files: [src/auth/callback.ts, tests/auth/callback.test.ts]
      shared_helpers: [src/auth/state.ts]
      fixtures_or_databases: []
      resources: []
      state: []
      ordering: [slice-1]
      independence_evidence: []
      serialization_reasons:
        - "Callback tests read state.ts; wait for slice-1's unfinished shared-worktree changes to settle"
        - "Same-slice Builder waits for completed extend authoring and delivered handoff"
    acceptance:
      - "Invalid state returns an authorization error"
    test_subtask:
      action: extend
      file: tests/auth/callback.test.ts
      cases:
        - "invalid state returns an authorization error"
  - task_id: slice-3
    description: "Document callback state errors"
    files: [docs/auth.md]
    level: easy
    depends_on: [slice-2]
    parallel_eligible: true
    conflict_checks:
      files: [src/auth/callback.ts, docs/auth.md]
      read_files: [src/auth/callback.ts, docs/auth.md]
      write_files: [docs/auth.md]
      shared_helpers: []
      fixtures_or_databases: []
      resources: []
      state: []
      ordering: [slice-2]
      independence_evidence: []
      serialization_reasons:
        - "Documentation reads callback behavior; wait for slice-2 to complete"
    acceptance:
      - "Documentation describes the authorization error for invalid state"
    test_subtask:
      action: none
      file: null
      cases: []
      reason: "Documentation-only slice; verify its wording against the completed callback behavior"
regression_tests: []
assumptions: []
```

`depends_on` defines slice readiness: a task with no dependencies is ready when
the full Plan is approved, and a task with dependencies is ready only after all
listed tasks complete. Every slice explicitly declares `test_subtask.action`.
For `create` or `extend`, that slice's Builder always waits for completed Test
Engineering and its delivered handoff, regardless of disjoint test/production
files or claimed independence from test output. The handoff identifies its
approved slice, role, scope, changed test paths, acceptance-criteria coverage,
and execution results or an explicit not-run statement. Failed, blocked, or
incomplete test authoring does not release the Builder. Completed authoring
may include expected red tests for missing behavior; passing tests are not a
prerequisite for implementation. `action: none` adds no Test Engineer stage;
ordinary approved dependencies and conflict checks still apply.

`parallel_eligible` permits overlap only for Test Engineer and Implementer
invocations from different slices after approval, readiness, and a recorded
independence assessment in `conflict_checks` or an adapter-native equivalent.
Assess approved scopes, read/write file use, shared helpers, fixtures/databases,
resources, mutable state, and ordering. `files` covers the complete scheduled
file scope, including `test_subtask.file` and files observed by tests/checks;
`read_files` and `write_files` distinguish their uses. Record evidence that each
proposed concurrent pair is independent, including whether tests/checks may
observe another slice's unfinished changes in the default `in_place_uncommitted`
shared worktree. Task IDs, separate outputs, or disjoint write paths alone do
not establish independence. When independence/conflicts are unknown or a conflict
exists, serialize affected invocations and record the reason in
`serialization_reasons` or equivalent existing run state. Proven independent
slices may overlap without requiring worktrees. The example's declared
dependencies serialize its slices; `parallel_eligible: true` alone grants no
overlap.

Keep every invocation and result attributable to its approved slice, role,
scope, and handoff; concurrency never combines acceptance criteria or
responsibility. Verifier, Reviewer, Knowledge Curator, and other specialists
never overlap active specialist work; verification follows all test/build work,
then review and curation run serially. These fields describe the contract and do
not implement a scheduler.

For record changes, use existing task files, acceptance criteria, assumptions, and
scope prose to state purpose, canonical home, owner, and approved writes. Link
relevant canonical documents and evidence for duplicate/contradiction checks;
keep normative requirements distinct from learned evidence. Task Results and
Knowledge Outcomes report actual scoped changes and unresolved issues through
their existing summary, evidence, concerns, reason, and remaining-work records as
applicable. Curation does not grant general project-documentation write permission.

## Task Result

```yaml
task_id: slice-1
role: implementer       # test_engineer | implementer
approved_scope: "src/auth/state.ts; five-minute expiry and single-use state"
test_handoff_ref: "slice-1/test_engineer/attempt-1" # logical result reference; null for action none
status: completed       # completed | failed | blocked | needs-replan
attempt: 1
retry:
  is_retry: false
  retry_of_attempt: null
  reason: null
recovery_state: pass     # pass | retry_implementer | replan | escalate | user_input
files_changed:
  - src/auth/state.ts
tests_run:
  - tests/auth/state.test.ts
summary: "Added expiring, single-use server-side state storage"
concerns: []
root_cause: null
```

Use this Task Result contract for Test Engineer handoffs as well as Builder
results, or an adapter-native equivalent preserving the same attribution.
`task_id`, `role`, and `approved_scope` identify the invocation; do not merge
criteria or responsibility across slices. A Test Engineer's `status` describes
authoring completion separately from test success. Its handoff uses
`files_changed` for changed test paths, `summary` for the acceptance-criteria
coverage, and `tests_run` for execution results or an explicit not-run statement.
Expected red tests for missing behavior may accompany `status: completed`;
failed, blocked, or incomplete authoring cannot release the same slice's Builder.
The Main Agent must deliver the completed result to the Builder before it starts.
A Builder's `test_handoff_ref` identifies that exact delivered result. A Test
Engineer sets this reference to `null` for its own outgoing handoff. For
`action: none`, the Builder sets it to `null` and explains the absence of a Test
Engineer stage in `summary`; approved dependencies/conflicts still apply.

## Verification Result

```yaml
status: passed          # passed | failed | blocked
affected_task_id: slice-1
affected_slice: slice-1
failure_classification: null  # observed failure: implementation_defect | test_failure | acceptance_failure | lint_failure | build_failure | check_failure | verifier_failure | invalid_plan | scope_change | dependency_invalidation | permission_failure | platform_failure | null
attempt: 1
retry_of_attempt: null
reentry_context: null  # example: {reason: verification_test_failure, prior_attempt: 1}
replan_trigger: null   # optional: null | invalid_plan | scope_change | acceptance_criteria_change | dependency_invalidation | permission_invalidation | platform_invalidation
evidence: []
recommended_action: pass  # retry_implementer | replan | escalate | user_input | pass
checks:
  - name: unit-tests
    status: passed
    command: "npm test -- state.test.ts"
    summary: "8 passed"
failures: []
acceptance_results:
  - criterion: "State is single-use"
    status: passed
```

`failure_classification` describes the observed failure. `replan_only_on` in the
Plan execution policy describes routing triggers; it is not another failure
classification. When a failure maps to a re-plan trigger, Verification Result
may include the optional `replan_trigger` field. Use `permission_failure` →
`permission_invalidation`, `platform_failure` → `platform_invalidation`, and a
criteria conflict → `acceptance_criteria_change`. `replan_trigger` is otherwise
`null` and may contain only one of the trigger names listed in its schema comment.

## Review Result

```yaml
status: changes-requested # approved | changes-requested | blocked
reviewed_scope_ref: "approved Plan/slice or full PR diff at recorded head/base"
evidence: []            # inspected artifacts, checks, and exact revision references
findings:
  - severity: medium
    disposition: required  # required | optional
    title: "Concurrent callbacks can both consume the same state"
    file: src/auth/state.ts
    line: 42            # null when no meaningful line location applies
    evidence: ["state-read/write implementation and concurrent-callback reproduction"]
    scenario: "Two callbacks read unused state before either marks it consumed"
    impact: "Both callbacks can pass a single-use authorization check"
    recommendation: "Make consumption atomic and cover concurrent submission"
unassessed_coverage: [] # important areas not assessed, with reasons and impact on decision
limitations: []
```

Findings require evidence, a reachable scenario, concrete impact, and a location
(including a line where applicable). `disposition` distinguishes required fixes
from optional suggestions. `changes-requested` needs at least one required
finding with supported impact; style or preference alone cannot block approval.
Report important unassessed coverage and limits even when no findings exist.
If material missing information prevents a decision, use `blocked`; do not
invent an approval or a defect. Existing statuses retain their meanings.

## PR Operation Result

This optional contract is both the operation's resumable checkpoint and its
result. It is independent of the development lifecycle and imposes no storage
path or new role. Adapters may use existing approved run state or an equivalent
record while preserving the identity, history, and fields below.

```yaml
operation_id: pr-operation-123
action: open                    # open | ship
status: pending                 # pending | completed | blocked | stopped
started_at: "YYYY-MM-DDTHH:MM:SSZ"
updated_at: "YYYY-MM-DDTHH:MM:SSZ"
identity:
  host: "resolved host"
  base_repository: "resolved owner/repository"
  base_branch: "resolved target branch"
  head_repository: "resolved owner/repository"
  head_branch: "resolved source branch"
  fork_identity: "resolved fork relationship, or explicit same-repository"
pr:
  url: null
  state: unresolved             # unresolved | absent | open | closed | merged
observed_revision:
  head_sha: null
  base_sha: null
  observed_at: null
publication_scope:
  selected_topics: []
  excluded_topics: []
  full_diff_ref: null           # entire resultant diff against exact target, not just a run's files
  selected_local_work_ref: null # staged/unstaged/untracked selection and preserved unrelated state
  authorization_ref: null       # explicit user action and chosen scope/mutations
  allowed_mutations: []
source_run_refs: []             # optional provenance; multiple/finalized/no summary are valid
approved_slice_mappings: []     # repairs: Plan/slice/scope/authority refs; never inferred from file presence
mutations:
  completed: []                 # exact action, identity, resulting SHA/URL, time, evidence
  partial: []                   # observed side effects and reconciliation still required
review:
  result_ref: null              # existing Review Result for full PR diff
  head_sha: null
  base_sha: null
  evidence: []
checks:
  gate_status: pending          # pending | passed | failed | blocked | unknown | not_applicable
  coverage: []                  # actual assessed behavior, gaps, and remote limitations
  evidence:
    - name: "check name"
      location: remote         # local | remote
      required: true
      outcome: pending         # passed | failed | pending | skipped | neutral | absent | blocked
      raw_conclusion: null     # exact host conclusion; separate from gate acceptance and execution evidence
      head_sha: "observed head SHA"
      base_sha: "observed base SHA"
      tested_sha: null         # actual executed SHA; do not assume it equals head
      tested_revision_kind: test_merge # head | test_merge | local_snapshot | unknown
      snapshot_ref: null       # exact local state when uncommitted work was tested
      run_url: null
      check_url: null
      observed_at: "YYYY-MM-DDTHH:MM:SSZ"
      freshness: unknown       # current | stale | unknown
      evidence_ref: null
budgets:
  recorded_at_start: "YYYY-MM-DDTHH:MM:SSZ"
  development: []              # per approved slice: max_retries, attempts_used, retries_used, history_refs
  observation_wait: {limit: 10, used: 0, deadline: "YYYY-MM-DDTHH:MM:SSZ"}
  infrastructure_rerun: {limit: 1, used: 0}
wait_state:
  reason: null
  next_observation_at: null
repair_result_refs: []         # existing Task/Verification/Review/Run Summary/Knowledge Outcome attribution
blockers: []
limitations: []
next_permitted_action: "resolve identity and chosen publication scope"
resume_checkpoint: "last reconciled state, pending work, and evidence to refresh"
```

All top-level fields are required except `source_run_refs`, which may be omitted.
Lists may be empty and unresolved values may be `null` only when explicitly
reported as pending, blocked, stopped, or inapplicable; they cannot hide required
evidence for completion. Counts and deadlines above are illustrative, not
defaults: record finite operation budgets at start. `completed` for `open`
requires the exact open PR URL and selected publication, then stops. `completed`
for `ship` additionally requires approved current independent full-diff review
and required gates satisfied under actual host/repository rules, with raw check
conclusions, actual coverage, and remote limitations reported separately.
Completion satisfies this action's reporting contract;
it never implies automatic merge readiness or an unobserved remote pass.
`pending` records bounded waiting or an approval dependency only while operation
allowance remains; `blocked` records unavailable required material evidence,
exhausted observation count/deadline, or another unresolved barrier;
`stopped` records an explicit stop without claiming success. No status implies
merge or post-merge execution.

`publication_scope` records the entire resultant diff, topic selection, local
work selection, preserved unrelated state, and authority required by the
[PR policy](../SPEC.md#optional-pr-capability). Closed/merged PRs, no diff,
ambiguity, and partial effects need explicit outcomes. Reconcile actual
local/remote state and completed/partial mutations before retry; resume with
the same operation identity/checkpoint, current authority, exact PR/revision, and
cumulative counters without duplicating commits, pushes, or PRs. A new invocation
does not reset any allowance.

Every check records the head/base it concerns, actual tested SHA (including a
test-merge SHA where applicable), URLs for remote runs/checks, outcome, time,
freshness, and supporting evidence. A local uncommitted check identifies its
exact snapshot instead of inventing a commit SHA. Unknown or unavailable values
must be explained in `limitations`/`blockers`. Head/base updates stale affected
review/check evidence; refresh it before proceeding. Absent/skipped/stale evidence
cannot prove that behavior executed and passed. Record raw conclusions, current
host/repository gate acceptance, and execution coverage separately: an accepted
skipped/neutral conclusion or conditional-job success may satisfy a gate without
executing the relevant behavior. Missing required checks still block completion.
`not_applicable` requires evidence that no gate applies; unknown requirements
remain `unknown` or `blocked`, never passed.

Keep queued, pending, absent, cancelled, timed-out, action-required, failed,
unknown, and success observations explicit in `checks.evidence`: preserve exact
host status in `evidence_ref` and exact conclusion in `raw_conclusion` (possibly
`null` before a conclusion), using an explained broader `outcome` where needed.
Raw host state, `checks.gate_status`, `checks.coverage`, and operation `status`
are separate claims. A defect handoff through `evidence_ref` and existing result
references supplies exact PR/current head/base/tested revision, run/check/job
identities and available URLs, relevant failed-log evidence, classification,
approved slice/write authority, and cumulative attempt/retry history. Explain
material gaps; never invent SHAs, logs, or authority. No parallel artifact or new
mandatory field is needed.

Before repair, `approved_slice_mappings` identifies the approved Plan, slice,
scope, and authorization evidence even when there are no source Run Summaries.
`budgets.development` carries cumulative slice attempts/retries and history
across invocations, sessions, and all relevant runs; neither republishing nor a
new session resets it. Observation/wait and infrastructure reruns have separate
finite counters, not development retries. Classify unknown, transient, permission,
or platform failures before treating them as code defects. A budget change
requires recorded authority and retains usage/history. Re-entry records use the
exact existing `workflow.reentry` trigger and evidence; a material Plan change
waits for renewed approval, then resumes this operation.

Main Agent records bounded foreground watch/poll progress in existing
`checks.evidence`, `budgets.observation_wait` (`limit`, `used`, `deadline`),
`wait_state` (`reason`, `next_observation_at`), blockers, next permitted action,
and resume checkpoint. Return the exact PR URL when present, last observation,
reason, and next action when an invocation ends. Manual resume suffices;
`next_observation_at` records a planned observation, not an installed wake-up.
Optional background/event/webhook capability must be discovered, configured, and
authorized; CI workflow creation proves none.

Use existing development artifact contracts for conditional Test Engineer
handoff, Builder, Verifier, independent Reviewer, Curator, and current repair
reporting. Reference those results (including Knowledge Outcome and any current
Run Summary) in `repair_result_refs`; do not redefine them or rewrite completed
run histories. Publication of a repair requires verified authorized work on the
same PR and renewed observation of its resulting revision.

## Knowledge Outcome

The full workflow requires a post-review Knowledge Curator assessment on every
run, without separate per-run opt-in. This contract also applies to independent
partial adoption of knowledge management, without requiring agents, models, or
run-state storage. Partial scopes without knowledge report `outside_scope`.

```yaml
applicable: true
status: no_change          # updated | no_change | blocked | outside_scope
reason: "Assessment found no eligible new durable learning beyond existing knowledge"
evidence: []               # references to assessed findings and relevant existing knowledge
files_changed: []          # completed writes only, including partial writes if blocked
checks:
  - status: not_applicable
    reason: "No knowledge writes were needed; no configured read-only checks apply"
remaining_work: []         # required, nonempty when blocked
```

All seven fields (`applicable`, `status`, `reason`, `evidence`, `files_changed`,
`checks`, `remaining_work`) are mandatory; lists may be empty only as their
conditions below allow. `reason` is mandatory and nonempty for every status. `applicable` is true for the
full workflow and partial scopes including knowledge management; it is false only
for an adoption scope without knowledge, which must use `outside_scope` and state
that adoption boundary in `reason`. An applicable assessment uses only `updated`,
`no_change`, or `blocked`; full workflow cannot claim `outside_scope`.

`updated` requires completed eligible, evidence-backed writes to the approved
configured system and passed applicable checks. Read relevant existing knowledge,
update or merge before creating redundant material, and preserve native mapping,
provenance, category protection, and approval safeguards. Record check results;
if no checks apply, explain that explicitly. `no_change` requires an actual
assessment with a reason such as duplicate, temporary, unsupported, code-obvious,
or otherwise no durable value. Empty writes alone do not establish assessment.
Missing approved paths, ambiguous write rules, failed writes, failed applicable
checks, unavailable required checks, or inability to assess required evidence are `blocked`, with the blocker
in `reason` and concrete `remaining_work`. Never invent destinations or report
blocked retention as `no_change`. Record completed partial writes separately;
they do not permit an `updated` outcome while required retention or checks remain.

Knowledge `checks` is a nonempty list of mappings with `status` and nonempty
`reason`. Allowed statuses are `passed`, `failed`, `blocked`, `not_run`, and
`not_applicable`. `passed` and `failed` also require the exact `command` and
supporting `evidence` references to retained output. Do not invent results.
`not_run` cannot satisfy an applicable required check; report the overall outcome
as `blocked` until it runs. `not_applicable` needs an explicit scope/configuration
reason; unavailability is not inapplicability. Check after the final topic,
metadata, navigation, and log write; an earlier check cannot certify a later write.
Mechanical success still requires substantive evidence/decision review. Keep
completed partial writes distinct from remaining work.

## Run Summary

```yaml
run_id: run-123
ticket_id: task-123
status: completed
files_changed:
  - src/auth/state.ts
  - src/auth/callback.ts
tests:
  passed: 20
  failed: 0
review: approved
reentry:
  intake_count: 1
  planner_count: 1
retries:
  total: 0
  by_task: {}
parallel_groups: []
serialized_conflicts: []
replan_reasons: []
native_discovery:
  status: verified             # verified | unknown | blocked
  discovery_id: discovery-123
  platform: codex
  platform_version: "release-or-channel"
  operating_mode: local
  host_runtime:
    name: runtime-name
    version: "runtime-version"
  source_records:
    - source_id: source-1
      status: verified       # verified | unknown | blocked
      source_url: https://first-party.example/docs/native-conventions
      source_path: null
      accessed_date: YYYY-MM-DD
      applicable_platform_version: "release-or-channel"
      applicable_operating_mode: local
      finding: "The host loads the verified native mechanism"
      confidence: high
      verification_evidence_ref: source-1
  native_destinations:
    - capability: main_agent_bootstrap
      status: verified       # verified | unknown | blocked
      exact_native_destination: "exact verified destination"
      mechanism: "exact load, invoke, or enforcement mechanism"
      verification_evidence_ref: source-1
      evidence_refs: [source-1]
  unknown_conventions: []
  stop_block_reasons: []
approval:
  status: approved            # pending | approved | ask_or_adjust | declined
  decision: proceed            # proceed | ask_or_adjust
  plan_id: plan-123
  recorded_at: YYYY-MM-DD
native_post_write_verification:
  required: true
  status: verified             # verified | unknown | blocked
  records:
    - capability: main_agent_bootstrap
      status: verified          # verified | unknown | blocked; per destination
      exact_native_destination: "exact verified destination"
      mechanism: "native diagnostic, effective-config view, registration/API result, or session check"
      verification_evidence_ref: verification-1
      evidence_id: verification-1
      source_url: null
      source_path: "/exact/path/to/verification-output-or-record"
      accessed_date: YYYY-MM-DD
      applicable_platform_version: "release-or-channel"
      applicable_operating_mode: local
      finding: "Host recognized, loaded, invoked, or enforced the destination"
      confidence: high
      result: verified
  limitations: []
knowledge_outcome:           # required; follows the Knowledge Outcome contract
  applicable: true
  status: no_change
  reason: "Assessment found no eligible new durable learning beyond existing knowledge"
  evidence: []
  files_changed: []
  checks:
    - status: not_applicable
      reason: "No knowledge writes were needed; no configured read-only checks apply"
  remaining_work: []
knowledge_updates: []        # completed knowledge writes; [] alone does not prove assessment
unresolved_risks: []
```

`native_discovery.status`, `unknown_conventions`, and `stop_block_reasons`
report discovery gaps without inventing a destination. `approval` records the
human decision that authorizes the approved Plan. `native_post_write_verification`
is required for every written native destination; every record has its own
`status` and canonical evidence fields. Its overall status is `verified` only
when every written destination has verified evidence. If any record is `unknown`
or `blocked`, the Run Summary must use that status, stop before additional target
writes, and must not claim that the destination was successfully recognized,
loaded, invoked, or enforced. Discovery or mapping with `unknown` or `blocked`
status also forbids `approval.decision: proceed` and target writes. All existing
Run Summary fields retain their prior meanings, including `reentry`, `retries`,
`parallel_groups`, `serialized_conflicts`, and `replan_reasons`.

`knowledge_outcome` is required and uses the Knowledge Outcome contract above.
Include completed writes in `knowledge_updates`, keeping it consistent with
`knowledge_outcome.files_changed`; partial completed writes may be listed for a
`blocked` outcome, with remaining work reported explicitly. An empty list is
valid with a reasoned `no_change`, a `blocked` outcome with no completed writes,
or an explicit `outside_scope` for partial adoption without knowledge. It never
proves assessment by itself. A blocked required knowledge outcome prevents the
Run Summary from claiming `status: completed`. Do not write run-specific learning
to this source repository's example knowledge tree by default.
