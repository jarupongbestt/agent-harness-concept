# Changelog

## [Unreleased]

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
