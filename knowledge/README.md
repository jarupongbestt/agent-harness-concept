# Knowledge-Base Integration

The harness treats the knowledge base as persistent project memory, separate from
temporary run state. This repository includes a small template and example tree so
that a clone has a usable starting shape.

## Canonical layout

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

`domain/` is the default project-domain area. A project may add more domain
folders under `knowledge/` when its scope requires them, while retaining
`main.md` and `log.md` at the knowledge root.

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

At the beginning of every run:

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
- Run-specific state belongs under `.harness/runs/`, not under durable knowledge.

## Finalization check

Before a non-trivial run ends, ask:

```text
Did this run teach a durable fact, constraint, decision, procedure, or recurring
failure pattern that would help a future task?
```

If yes, the Knowledge Curator updates the appropriate topic page, index, and log,
then runs the knowledge linter.
