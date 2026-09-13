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
tasks:
  - task_id: slice-1
    description: "Store OAuth state server-side"
    files:
      - src/auth/state.ts
    level: hard              # easy | hard
    depends_on: []
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
    acceptance:
      - "Invalid state returns an authorization error"
regression_tests: []
assumptions: []
```

## Task Result

```yaml
task_id: slice-1
status: completed       # completed | failed | blocked | needs-replan
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
knowledge_updates:
  - knowledge/auth/self/oauth-state.md
unresolved_risks: []
```
