# Portable Artifact Contracts

These are logical contracts. Adapters may represent them as JSON, YAML, Markdown,
or native tool results, but the fields and meaning should remain compatible.

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
  reentry_policy:
    intake:
      default_passes: 1
      allowed_triggers:
        - user_clarification_changes_ticket
        - newly_discovered_ticket_mismatch
    planner:
      default_passes: 1
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
    parallel_eligible: true   # eligible only when ready and non-conflicting
    conflict_checks:
      files:
        - tests/auth/state.test.ts # test_subtask.file is part of conflict scope
      resources: []           # shared mutable resources require serialization
      state: []               # conflicting mutable state requires serialization
      ordering: []            # explicit ordering constraints require serialization
    acceptance:
      - "State expires after five minutes"
      - "State is single-use"
    test_subtask:
      action: create          # create | extend | none
      file: tests/auth/state.test.ts
      cases:
        - "expired state is rejected"
  - task_id: slice-2
    description: "Validate state in the callback"
    files:
      - src/auth/callback.ts
    level: easy
    depends_on: [slice-1]
    parallel_eligible: true
    conflict_checks:
      files: []
      resources: []
      state: []
      ordering: [slice-1]
    acceptance:
      - "Invalid state returns an authorization error"
regression_tests: []
assumptions: []
```

`depends_on` defines readiness: a task with no dependencies is ready when the
plan is approved, and a task with dependencies is ready only after all listed
tasks complete. `parallel_eligible` permits concurrent scheduling only after
readiness and conflict checks pass. Overlapping files, shared resources,
conflicting mutable state, or ordering constraints serialize the affected tasks;
`test_subtask.file` is part of the slice's scheduled file scope and must be
included in `conflict_checks.files` or equivalent conflict analysis. These fields
describe the contract and do not implement a scheduler.

## Task Result

```yaml
task_id: slice-1
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
status: approved        # approved | changes-requested | blocked
findings:
  - severity: medium
    title: "Missing concurrent callback test"
    file: tests/auth/callback.test.ts
    recommendation: "Add a test for duplicate callback submission"
```

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
knowledge_updates:
  - knowledge/auth/self/oauth-state.md
unresolved_risks: []
```
