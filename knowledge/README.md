# Knowledge-Base Integration

The harness treats the knowledge base as persistent project memory, separate from
temporary run state.

## Skill ownership

The knowledge base should own its own portable `knowledge-base` skill. That skill
defines how an agent navigates indexes, distinguishes internal knowledge from
external sources, records provenance, handles contradictions, updates the action
log, and validates the knowledge tree.

The harness does not require a vendor-specific knowledge skill or runtime. It only
defines when the workflow reads knowledge and when the Knowledge Curator may update
it. The knowledge-base repository may implement those rules as a skill for Claude,
Codex, OpenCode, or another platform, with thin platform adapters pointing to the
same portable instructions.

## Read path

```text
knowledge/main.md
    ↓
domain/index.md
    ↓
self/<topic>.md or derived/<topic>.md
    ↓
specific source file when required
```

At the beginning of a run:

1. Main Agent reads `knowledge/main.md`.
2. Main Agent reads recent entries from `knowledge/log.md`.
3. Intake uses navigation hints to identify likely scope.
4. Planner reads the matched topic pages and source references.

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
