# Skill Origins and Provenance

This registry explains where the skill concepts came from. It is deliberately
explicit because the harness is intended to be copied or adapted by Claude, Codex,
OpenCode, Hermes, and other agents.

The skills are not claimed to be copied verbatim from any one runtime. `Adapted`
means the concept is based on a named source and rewritten for the portable
harness. `Synthesized` means it combines the referenced harness design with common
software-engineering practice. `Domain extension` means it is a placeholder for a
project-specific capability, not a skill supplied by this repository.

The table below records historical concept provenance: why this repository has a
skill and what it retains. It does not select the source to materialize in an
adopter project. That per-application choice belongs in the application artifact's
`materialization_source` and must follow the candidate registry in
[`../harness.yaml`](../harness.yaml). A matching upstream slug is only a candidate;
it does not establish authorship, origin, equivalence, or compatibility.

## Materialization source policy

- Every selected skill gets exactly one `materialization_source`. Never install
  the local definition and an upstream candidate as duplicate copies of the same
  skill.
- The `addyosmani/agent-skills` `main` references in `harness.yaml` are mutable
  candidate refs, not pinned sources. At application time, inspect the selected
  source and record its resolved full commit SHA and access date in the
  application artifact. Do not treat a research-time SHA as an installation lock.
- Exact same-slug direct candidates are listed for `context-engineering`,
  `test-driven-development`, `security-and-hardening`, `documentation-and-adrs`,
  and `doubt-driven-development`. They are candidates, not presumed compatible.
- `source-driven-development` remains local by default. Its upstream same-slug
  candidate is a narrower optional method for framework-documentation decisions,
  not an automatic replacement.
- `clarification`, `specification`, `task-decomposition`, `root-cause`,
  `code-review`, and `incremental-implementation` remain local by default.
  `interview-me`, `idea-refine`, `spec-driven-development`,
  `planning-and-task-breakdown`, `debugging-and-error-recovery`, and
  `code-review-and-quality` are optional methods only, not automatic equivalents.
- `incremental-implementation` stays local because the upstream same-slug skill
  includes per-slice verify/commit behavior that conflicts with this harness's
  execution and no-commit rules.
- The harness lifecycle, approval gates, role boundaries, no-commit default,
  concurrency policy, and artifact contracts override generic upstream methods.
  If adopting an upstream body requires changing its content to meet those rules,
  record the materialization as a `local_adaptation` with its upstream influence
  in provenance; do not claim the changed body is a direct upstream copy.
- Inspect selected upstream skill folders for supporting files. Record required,
  included, and omitted assets. If a source is incompatible or unavailable, record
  the fallback source and reason, or record an explicit omission reason.

| Skill | Origin type | Origin source(s) | What this harness keeps |
|---|---|---|---|
| `context-engineering` | Adapted | [template-harness](https://github.com/jarupongbestt/template-harness) | Small, role-specific contexts and distilled state in the Main Agent |
| `specification` | Synthesized | [template-harness](https://github.com/jarupongbestt/template-harness); requirements-engineering practice | Tickets and acceptance criteria that can be checked |
| `clarification` | Synthesized | [template-harness](https://github.com/jarupongbestt/template-harness); interview and idea-refinement practice | Resolve material ambiguity; use concise interviewing and refine rough ideas only as needed |
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
| `karpathy-guidelines` | Synthesized | [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876); [portable skill reference](https://github.com/multica-ai/andrej-karpathy-skills/tree/main/skills/karpathy-guidelines) | Think before acting, choose the simplest sufficient solution, keep edits surgical, and verify observable outcomes; rewritten as a concise portable skill |
| `doubt-driven-development` | Adapted | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills/tree/main/skills/doubt-driven-development) | Main-Agent orchestration that sends a minimal packet for fresh-context adversarial review, reconciles evidence, and bounds follow-up; no mandatory external CLI/model |
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
