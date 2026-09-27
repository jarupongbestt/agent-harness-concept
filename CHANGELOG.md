# Changelog

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
