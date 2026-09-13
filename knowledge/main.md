# Knowledge Navigation

This file is the mandatory startup read for the durable project knowledge base.
It contains navigation rules and stable entry points, not a task transcript.

## Entry points

- [`domain/index.md`](domain/index.md) — default project-domain index.
- [`_template/`](_template/) — copyable topic templates.
- [`_example/`](_example/) — non-authoritative example entries.
- [`log.md`](log.md) — action history, read only when historical or lint context
  is needed.

## Navigation rule

Read the matching domain index first, then only the relevant `self/`, `derived/`,
or `sources/` entry. Do not scan the entire knowledge tree by default.

## Write rule

Durable discoveries go in a topic page. Update an index when navigation changes.
Record knowledge changes in `log.md`, then run the knowledge linter when one is
available.
