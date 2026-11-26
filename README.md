# Portable Agent Harness

Portable Agent Harness defines a plan-first workflow for software development
with AI agents. The Main Agent coordinates the run, gets your approval, and
delegates work to specialist subagents. Adapters map the same workflow to the
native tools of Codex, Claude Code, Pi, OpenCode, Hermes, and other platforms.

## Quick start

Give your agent a link to this repository and ask it to apply the harness to
your project. Choose the full workflow, selected roles, or a capability such
as knowledge management.

Ask the agent to follow the [application guide](APPLY.md). It should inspect
your project, verify what your platform supports, and get your approval for
the proposed setup before editing files.

## Workflow

The Main Agent answers ordinary questions directly and delegates execution
requests through these stages.

1. **Intake and planning.** Intake turns the request into a Ticket. The Planner
   uses it to write the complete Plan.
2. **Approval.** You approve the full Plan before any project files change.
3. **Implementation.** When a slice needs new or extended tests, the Test
   Engineer finishes and hands them off before the Builder starts that slice.
   The Builder implements the approved changes.
4. **Verification and review.** Once test authoring and implementation finish,
   the Verifier runs checks and the Reviewer independently assesses the changes.
5. **Knowledge and Finalize.** In the full workflow, the Knowledge Curator
   assesses what the team learned and retains useful, evidence-backed findings.
   The Main Agent summarizes the outcome and finalizes locally.

## Optional capabilities

Select these capabilities separately from the full workflow.

- [`frontend-ui`](APPLY.md#frontend-ui-extension) adds guidance for the Builder
  and Reviewer when they work on browser-facing changes.
- [`pr`](SPEC.md#optional-pr-capability) adds pull request publication when you
  explicitly authorize it.

## Reference

- [Workflow rules and policies](SPEC.md)
- [Main Agent and delegation instructions](AGENTS.md)
- [Platform adapter requirements](adapters/README.md)
- [Artifact formats](schemas/artifacts.md)
