# Knowledge-Base Integration Example

The harness treats a knowledge base as persistent project memory, separate from
temporary run state. The layout below describes this repository's example
knowledge system. It is not a required target-project layout. When applying the
harness elsewhere, inspect and preserve the target's existing knowledge root,
navigation, categories, and protection rules. Use a different layout when the
target already has one; propose a new location only when needed and approved.

## Example layout in this repository

```text
knowledge/
├── main.md                 # mandatory navigation read at startup
├── log.md                  # action log; read only for history, audit, or lint
├── domain/
│   ├── index.md            # domain navigation
│   ├── self/               # internal discoveries and decisions
│   ├── derived/            # compiled external knowledge
│   └── sources/            # raw external material, when used
├── _template/              # copyable topic templates
└── _example/               # non-authoritative example entries
```

Here, `domain/` demonstrates one way to separate domain knowledge from the
navigation and log files. It is not a universal domain directory or a default
destination for applied projects. Do not create `knowledge/domain/` in a target
unless inspection shows that it fits the target and the user approves the map.

## Read path

```text
knowledge/main.md
    ↓
knowledge/domain/index.md
    ↓
knowledge/domain/self/<topic>.md or knowledge/domain/derived/<topic>.md
    ↓
knowledge/domain/sources/<file> when required
```

For this repository's workflow, at the beginning of every run:

1. The Main Agent reads `knowledge/main.md`.
2. Intake uses its navigation hints to identify likely scope.
3. Planner reads the matched domain index, topic pages, and source references.

`knowledge/log.md` is not a mandatory startup read. Read it only when the task
needs historical activity, contradiction analysis, recurring-failure context,
audit evidence, or lint diagnostics. The log records knowledge actions and is
validated by the knowledge linter; it is not a transcript or startup memory dump.

## Write rules

- `sources/` is raw external input and should be protected from normal edits.
- `derived/` is a faithful compilation of external sources and is locked until
  explicitly re-synced.
- `self/` contains internal discoveries, decisions, constraints, and gotchas.
- Contradictions are recorded and surfaced; they are not silently overwritten.
- Indexes contain navigation, not detailed knowledge.
- The action log records knowledge changes, not complete task transcripts.
- Run-specific state belongs in the separately approved/discovered run-state
  location, not under durable knowledge. `.harness/runs/` is an example used by
  this repository; it is not imposed on target projects.

## Finalization check

When knowledge management is selected, before a non-trivial run ends, ask:

```text
Did this run teach a durable fact, constraint, decision, procedure, or recurring
failure pattern that would help a future task?
```

If yes, the Knowledge Curator updates the appropriate topic page, index, and log,
then runs the knowledge linter.
