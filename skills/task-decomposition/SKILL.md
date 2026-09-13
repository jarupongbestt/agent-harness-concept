---
name: task-decomposition
description: Split an approved change into dependency-aware, scoped, independently verifiable task slices.
---

# Task Decomposition

Create slices that one Implementer invocation can complete safely.

- Give each slice one objective, explicit files, acceptance criteria, and test
  action.
- Declare dependencies, overlapping files, shared resources, mutable state, and
  ordering constraints.
- Mark independent ready slices eligible for parallel execution.
- Serialize only the affected slices when a conflict or dependency exists.
- Keep planning separate from implementation and preserve the approval boundary.

Do not split merely by file if doing so breaks a behavior boundary or makes a
slice unverifiable.
