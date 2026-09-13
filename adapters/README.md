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

## Native Convention Discovery

Native Convention Discovery is an adapter responsibility and a mandatory,
read-only prerequisite for every adapter/application mapping. Complete it
before concept mapping, proposing a target file map, or any write. The adapter
must inspect the actual host and target repository and report the detected
platform, platform version or release/channel, operating mode, host runtime and
version, and host capabilities relevant to the mapping. It must also inspect
the target's existing instructions, configuration, agent/session mechanisms,
skills, checks, knowledge paths, and applicable project constraints.

The discovery report must use current, mutually consistent first-party
evidence. For each material finding, record a first-party source URL (or exact
first-party source path where a URL is not applicable), access date, applicable
platform version and operating mode, finding, confidence, and
`verification_evidence_ref`. For every portable capability that the mapping will
`reuse` or `translate`, record the exact native destination and the mechanism by
which the host loads, invokes, or enforces it, but only when that destination and
mechanism are verified.

Use these canonical fields for each auditable finding and keep them consistent
with `schemas/artifacts.md`:

```yaml
capability: specialist_delegation
status: verified                 # verified | unknown | blocked
source_url: https://first-party.example/docs/native-conventions
source_path: null                # exactly one of source_url/source_path
accessed_date: YYYY-MM-DD
applicable_platform_version: "release-or-channel"
applicable_operating_mode: local
finding: "The host supports isolated specialist sessions"
confidence: high
exact_native_destination: "exact verified native destination"
mechanism: "exact load, invoke, or enforcement mechanism"
verification_evidence_ref: "source-1 or exact verification command/record"
```

Exactly one of `source_url` or `source_path` is required; `source_path` must be
an exact host or first-party path. `applicable_platform_version`,
`applicable_operating_mode`, `finding`, and `confidence` are required. Every
destination has its own `status`, and discovery is `verified` only when every
relevant destination and evidence record is verified.

Unknown, unsupported, contradictory, stale, or low-confidence conventions must
not be guessed or promoted into a file map. Mark the affected capability
`unknown` or `blocked`, stop before mapping, approval to proceed, or writing,
and ask the user for the missing information or report the evidence gap. In
particular, a path that exists and an example from this repository are not
evidence that a host supports a native convention. A discovery, concept map, or
file map with `unknown` or `blocked` status forbids `approval.decision:
proceed` and target writes.

The examples `.claude/`, `.codex/`, `.agents/`, `CLAUDE.md`, and `AGENTS.md` are
illustrative and non-normative only; they are never evidence, defaults, or
universal destinations. The same applies to every path in the illustrative
mapping below. An example may be used only after independent discovery verifies
the actual host's destination and mechanism.

The application output must be a native projection. If a proposed target tree
looks like a copy of this repository—for example, it adds root `prompts/`,
`schemas/`, `knowledge/`, or `skills/` solely because those directories exist
here—stop before approval and return a failed source-tree-copy audit. Translate
role prompts into the host's native agent or skill format, or reference them as
source material without copying them.

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

Platform path examples (illustrative and non-normative; verify before use):

```text
Claude   → CLAUDE.md, .claude/agents/, and .claude/skills/
Codex    → AGENTS.md, .codex/agents/, and .agents/skills/
OpenCode → .opencode/ agents, tools, and plugins
Hermes   → Hermes skills and tool configuration
```

For the current evidence behind these examples, consult the official
[Codex instruction guidance](https://learn.chatgpt.com/docs/agent-configuration/agents-md),
[Codex subagent guidance](https://learn.chatgpt.com/docs/agent-configuration/subagents),
and [Codex skills guidance](https://learn.chatgpt.com/docs/build-skills), plus
Claude Code's [instruction guidance](https://code.claude.com/docs/en/memory),
[skills guidance](https://code.claude.com/docs/en/skills), and
[subagent guidance](https://code.claude.com/docs/en/agents). These links do not
replace per-run discovery and native post-write verification.

## Native post-write verification

After an approved mapping is written, the adapter/application must verify
natively that each written destination and mechanism is recognized, loaded,
invoked, or enforced by the host. File existence alone is insufficient. Use the
host's native diagnostic, effective-configuration view, registration or API
result, session/delegation check, or equivalent mechanism, as applicable.

Record exact verification evidence for every destination using a per-destination
record with `status: verified`, `unknown`, or `blocked`; `exact_native_destination`,
`mechanism`, `verification_evidence_ref`, source URL or exact source path,
`accessed_date`, `applicable_platform_version`,
`applicable_operating_mode`, `finding`, and `confidence`. The overall result is
`verified` only when every written destination has a verified record and complete
evidence. If native verification cannot be completed or evidence fails, mark the
affected record `unknown` or `blocked`, stop before additional target writes, and
do not claim success.

The adapter must not redefine the workflow or introduce a different meaning for
Ticket, Plan, approval, verification, or knowledge provenance. It must not imply
that the Main Agent performs all specialist work itself when the host supports
subagents. The adapter is execution mapping only: it must not redefine the
portable lifecycle or imply one fixed directory or file layout.
