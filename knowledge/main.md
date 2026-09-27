# Knowledge Navigation Example

This file is the startup entry point for this repository's durable knowledge
base. It contains navigation rules and stable entry points, not a task transcript.
When applying the harness to another project, preserve that project's existing
knowledge entry point and adapt these rules to its structure. Do not assume the
target has a `knowledge/` root or a `domain/` directory.

## Entry points

- [`domain/index.md`](domain/index.md) — this repository's example domain index.
- [`_template/`](_template/) — copyable topic templates.
- [`_example/`](_example/) — non-authoritative example entries.
- [`log.md`](log.md) — action history, read only when historical or lint context
  is needed.

## Navigation rule

For this repository, read the matching domain index first, then only the relevant
`self/`, `derived/`, or `sources/` entry. In an applied project, follow that
project's configured categories and index paths instead. Do not scan the entire
knowledge tree by default.

## Write rule

Durable discoveries go in a topic page. Update an index when navigation changes.
Record knowledge changes in `log.md`, then run the knowledge linter when one is
available.
