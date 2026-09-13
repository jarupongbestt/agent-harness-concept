# Portable Agent Harness Specification

Status: concept
Version: 0.1

## 1. Purpose

The Portable Agent Harness defines a common workflow for software-development
agents without depending on a particular agent runtime, model provider, or tool
protocol.

The harness standardizes:

- lifecycle and stage order
- agent responsibilities
- structured artifacts
- approval and safety gates
- model routing
- testing and review policy
- knowledge-base usage
- final run reporting

Platform adapters translate these rules into Claude, Codex, OpenCode, Hermes, or
another agent's native instructions and tools.

## 2. Design principles

1. **Main Agent owns orchestration; subagents own specialist work.** The Main
   Agent delegates work, holds distilled state, manages approval, and decides how
   to recover from failure. It must not be interpreted as the only agent in the
   workflow.
2. **Plan before editing.** Every task that changes project behavior has a plan.
3. **Approval before implementation.** The user approves the plan before any
   implementation begins.
4. **Small isolated contexts.** Each specialist receives only the ticket, relevant
   knowledge, scope, and task-specific inputs.
5. **Skills are reusable.** Roles describe responsibility; skills describe methods.
6. **Mechanical checks are not LLM reasoning.** Running tests, linting, formatting,
   and schema validation should use scripts or native tools where possible.
7. **Least privilege.** An agent receives only the tools and write scope required
   for its role.
8. **The user owns irreversible version-control actions.** The default harness does
   not commit, push, merge, or create releases.
9. **Knowledge compounds.** Durable decisions and discoveries are recorded for
   future tasks; temporary session state is not treated as knowledge.

10. **English coordinates agents; the user language coordinates the conversation.**
    Internal reasoning, delegation payloads, role prompts, and subagent artifacts
    use English. User-facing responses follow the language of the request and may
    be hybrid when the user is multilingual.

The workflow is intentionally multi-agent. A conforming adapter must provide a way
to invoke specialist subagents, or explicitly report that the host lacks that
capability. A single-agent fallback may preserve the role boundaries and artifact
contracts, but it is a platform limitation—not the harness's default operating
model.

## 3. Workflow

### 3.1 User Request

The user provides a natural-language request. The Main Agent creates a new run and
assigns it a `run_id`.

### 3.2 Intake

The Main Agent delegates Intake to the Intake Agent. The Intake Agent reads the
knowledge root index and recent activity, then creates a Ticket. It identifies the
change type, scope, acceptance criteria, risk, confidence, and whether clarification
is required.

For bugfixes or unclear failures, Intake loads the `root-cause` skill and performs a
bounded investigation itself. It must distinguish evidence from assumptions.

### 3.3 Clarify

Clarify is a Main Agent responsibility. It asks the user only when the Ticket lacks
information needed to produce a reliable plan. No implementation or detailed
planning occurs while critical ambiguity remains.

### 3.4 Plan

The Main Agent delegates planning to the Planner Agent. The Planner reads the
Ticket, relevant knowledge pages, source references, and test-impact information. It
creates an ordered list of small task slices.

For bugfixes, the Planner loads `root-cause` when Intake's evidence is incomplete or
when the proposed change depends on an unverified cause.

Each slice includes files, dependencies, acceptance criteria, difficulty, and test
requirements.

### 3.5 User Approval

The Main Agent presents a concise, plain-language plan and asks:

```text
Proceed with this plan?

1. Proceed
2. Ask / adjust
```

If the user asks for changes, the Main Agent sends feedback to the Planner and
repeats the approval step. No agent may edit project files before approval.

### 3.6 Test Design

When required, the Main Agent delegates this stage to the Test Designer.

For a feature or bugfix without matching coverage, the Test Designer creates or
extends tests from the acceptance criteria. It should not depend on the
Implementer's implementation approach.

For refactors, existing tests are used as regression protection. Cosmetic changes
may use a smoke or visual check instead of a new unit test.

### 3.7 Implement

The Main Agent delegates one approved task slice at a time to the Implementer. The
Implementer changes only the
approved scope, follows the acceptance criteria, and returns a structured result.

If implementation reveals that the plan is incomplete or incorrect, the
Implementer stops and reports the discrepancy. The Main Agent then sends the work
back to Plan and, if necessary, reopens approval.

When a test fails, the Implementer loads `root-cause` before retrying. It must report
the cause it is addressing rather than repeatedly changing code based only on the
latest error message.

### 3.8 Verify

The Main Agent delegates verification to the Verifier Agent when the platform
supports that role as an isolated subagent. Verify is primarily mechanical. It runs
the narrowest useful checks first, followed by affected regression checks:

- unit and integration tests
- type checks
- lint and formatting checks
- build checks
- acceptance-criteria checks
- direct and dependent regression tests

The Main Agent decides whether a failure requires a retry, escalation, re-planning,
or user input.

### 3.9 Review

The Main Agent delegates review according to risk. The Reviewer independently checks the changes against the Ticket, approved Plan,
acceptance criteria, test results, security expectations, and scope.

Review depth is risk-based:

- low risk: scope and correctness review
- medium risk: edge cases and regression review
- high risk: independent context, security review, and test-quality review

### 3.10 Knowledge Update

The Knowledge Curator records durable information learned during the run:

- architectural decisions
- project conventions
- integration constraints
- recurring failure causes
- security requirements
- testing relationships
- operational procedures

It does not copy the conversation transcript into the knowledge base.

### 3.11 Finalize

The Main Agent confirms the final scope, test results, review findings, knowledge
updates, unresolved risks, and changed files. It returns control to the user.

## 4. Complexity routing

All tasks are planned. Complexity only controls model strength and review depth.

| Tier | Typical task | Routing |
|---|---|---|
| 0 | cosmetic or very small bounded change | junior Planner, junior Implementer, lightweight review |
| 1 | normal feature or refactor | standard Planner, task-level Implementer, normal review |
| 2 | cross-cutting, load-bearing, security, money, data, or low-confidence work | senior Planner, stronger Implementer, independent review |

When uncertain, route upward. The cost of a stronger model is usually lower than
the cost of a wrong plan or missed regression.

## 5. State and permissions

The Main Agent may hold:

- Ticket
- Plan
- approval status
- task results
- verification results
- review findings
- knowledge-update summary

The Main Agent should not carry large raw file contents between stages.

Suggested default permissions:

| Role | Read project | Write source | Write tests | Write knowledge | Git write |
|---|---:|---:|---:|---:|---:|
| Main Agent | yes | no or limited | no | delegated | no |
| Intake | scoped | no | no | no | no |
| Planner | scoped | no | no | no | no |
| Test Designer | scoped | no | yes | no | no |
| Implementer | scoped | yes | no | no | no |
| Verifier | scoped | no | no | no | no |
| Reviewer | scoped | no | no | no | no |
| Knowledge Curator | relevant | no | no | yes | no |

The exact enforcement mechanism belongs to the adapter.

## 6. Completion criteria

A run is complete when:

- the approved plan was executed or explicitly reported as blocked
- verification results are available
- review is complete or intentionally skipped with a reason
- scope was audited
- durable knowledge was considered
- no unauthorized git action occurred
- the Main Agent returned a final summary
