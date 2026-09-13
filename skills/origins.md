# Skill Origins and Provenance

This registry explains where the skill concepts came from. It is deliberately
explicit because the harness is intended to be copied or adapted by Claude, Codex,
OpenCode, Hermes, and other agents.

The skills are not claimed to be copied verbatim from any one runtime. `Adapted`
means the concept is based on a named source and rewritten for the portable
harness. `Synthesized` means it combines the referenced harness design with common
software-engineering practice. `Domain extension` means it is a placeholder for a
project-specific capability, not a skill supplied by this repository.

| Skill | Origin type | Origin source(s) | What this harness keeps |
|---|---|---|---|
| `context-engineering` | Adapted | [template-harness](https://github.com/jarupongbestt/template-harness) | Small, role-specific contexts and distilled state in the Main Agent |
| `specification` | Synthesized | [template-harness](https://github.com/jarupongbestt/template-harness); requirements-engineering practice | Tickets and acceptance criteria that can be checked |
| `clarification` | Adapted | [template-harness](https://github.com/jarupongbestt/template-harness) | Resolve material ambiguity before planning or editing |
| `root-cause` | Adapted | [template-harness](https://github.com/jarupongbestt/template-harness) investigation workflow | Evidence-first tracing from symptom to cause; it is a reusable skill, not a permanent agent |
| `task-decomposition` | Adapted | [template-harness](https://github.com/jarupongbestt/template-harness) | Ordered, small, independently verifiable task slices |
| `source-driven-development` | Adapted | [knowledge-base](https://github.com/jarupongbestt/knowledge-base) | Navigate relevant knowledge and cite evidence before making decisions |
| `incremental-implementation` | Adapted | [template-harness](https://github.com/jarupongbestt/template-harness) | One approved slice at a time, smallest coherent change, no silent scope expansion |
| `test-driven-development` | Synthesized | [template-harness](https://github.com/jarupongbestt/template-harness); test-design / acceptance-criteria practice | Tests are derived from behavior and are separated from the Implementer's design |
| `code-review` | Synthesized | [template-harness](https://github.com/jarupongbestt/template-harness); standard code-review practice | Independent review of correctness, scope, regressions, and test quality |
| `security-and-hardening` | Synthesized | [template-harness](https://github.com/jarupongbestt/template-harness); standard secure-development practice | Risk-based scrutiny of sensitive boundaries and unsafe assumptions |
| `documentation-and-adrs` | Adapted | [knowledge-base](https://github.com/jarupongbestt/knowledge-base) | Record durable decisions, constraints, procedures, and recurring failures |
| `harness-artifacts` | Synthesized | [agent-harness](https://github.com/jarupongbestt/agent-harness) | Preserve structured artifact contracts and evidence fields across roles |
| `knowledge-base` | Adapted | [knowledge-base](https://github.com/jarupongbestt/knowledge-base) | Navigate, update, and lint the durable knowledge tree |
| `frontend-ui` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `api-design` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `database-migrations` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `data-pipelines` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `cloud-infrastructure` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `observability` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |

## Provenance rules for future skills

Every new skill should add one row here with:

- an origin type
- a stable URL, file path, or explicit `project-specific` declaration
- a short description of what was adopted
- any meaningful rewrite or limitation

If a skill is derived from multiple sources, list all of them. If the sources
conflict, preserve the conflict for review instead of attributing a single source
to a blended rule.
