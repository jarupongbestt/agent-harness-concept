# Platform Adapter Requirements

An adapter makes the portable harness executable on a specific agent platform.
It is target-specific configuration and mapping only: it translates the
portable responsibilities into the target repository's native conventions. It
is not a copy, fork, installation, or reproduction of this repository.

The adapter is a mapping, not a copy of this repository's folder tree. The portable
concept does not require `.claude/agents/`, `.codex/agents/`, `.agents/skills/`, or
any other exact path. See [`../APPLY.md`](../APPLY.md) for an illustrative mapping
and the questions an applying agent must ask first.

The target may use zero, one, or multiple native configuration files, depending
on the actual platform capabilities and the target repository's existing setup.
Do not create adapter files merely to match the examples below; create or
modify only the approved target-specific files.

Each adapter must define how to perform these capabilities. **Delegation is
required by the portable design**: the adapter must map the specialist roles to
native subagents, agent sessions, or equivalent isolated execution contexts.

| Portable capability | Adapter responsibility |
|---|---|
| Main Agent bootstrap | Map the bootstrap contract into the target platform's native startup mechanism. [`../AGENTS.md`](../AGENTS.md) is source material only; it is never a target path to copy or load literally. |
| Read files | Map to the platform's file-reading mechanism |
| Write files | Map to scoped editing tools |
| Delegate | Spawn or invoke isolated role subagents |
| Ask user | Implement the approval gate |
| Run checks | Invoke shell, test, or validation tools |
| Permissions | Restrict tools and write paths by role |
| Structured output | Preserve the artifact contracts |
| Knowledge update | Allow only the Knowledge Curator write path |

Suggested adapter files (illustrative and non-normative; do not create these by
default):

```text
adapters/<platform>/
├── README.md
├── bootstrap.md
├── role-mapping.md
├── permissions.md
└── install-or-run.md
```

Platform path examples (illustrative and non-normative):

```text
Claude   → CLAUDE.md and .claude/
Codex    → AGENTS.md and Codex skill configuration
OpenCode → .opencode/ agents, tools, and plugins
Hermes   → Hermes skills and tool configuration
```

The adapter must not redefine the workflow or introduce a different meaning for
Ticket, Plan, approval, verification, or knowledge provenance. It must not imply
that the Main Agent performs all specialist work itself when the host supports
subagents.
