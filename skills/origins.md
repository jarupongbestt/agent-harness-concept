# Skill Origins and Provenance

This registry explains where the skill concepts came from. It is deliberately
explicit because the harness is intended to be adapted by Claude Code, Codex,
Pi, OpenCode, Hermes, and other agents.

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
  `test-driven-development`, and `doubt-driven-development`. They are candidates,
  not presumed compatible.
- `security-and-hardening` uses its single local definition as a
  `local_adaptation`. The `addyosmani/agent-skills` same-slug candidate is excluded
  from this method's source selection in favor of the reviewed local adaptation;
  matching names do not establish equivalent methods. See the
  [adaptation record](#security-and-hardening-adaptation).
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
| `security-and-hardening` | Local adaptation | [Cloudflare security-audit-skill](https://github.com/cloudflare/security-audit-skill/tree/d24bc269171a9171fac58493e0ffba511d571a4a); historical [template-harness](https://github.com/jarupongbestt/template-harness) and secure-development practice | Conditional, evidence-backed scrutiny of concrete affected boundaries, including private derived data; bounded validation and proportionate corrections within existing roles |
| `documentation-and-adrs` | Local adaptation | [knowledge-base](https://github.com/jarupongbestt/knowledge-base) | Record durable decisions, constraints, procedures, and recurring failures |
| `harness-artifacts` | Synthesized | [agent-harness](https://github.com/jarupongbestt/agent-harness) | Preserve structured artifact contracts and evidence fields across roles |
| `knowledge-base` | Adapted | [knowledge-base](https://github.com/jarupongbestt/knowledge-base) | Navigate, update, and lint the durable knowledge tree |
| `code-quality` | Local adaptation | [Ponytail](https://github.com/DietrichGebert/ponytail/blob/96608e078b2d5ea607c5c3b8b5a54d1c1fba6846/skills/ponytail/SKILL.md); [Minimize Reader Load](https://github.com/cursor/plugins/blob/bb68f2365eceafa0cb2d253112f675acf5f144cc/pstack/skills/principle-minimize-reader-load/SKILL.md); historical [Andrej Karpathy observations](https://x.com/karpathy/status/2015883857489522876) and [portable skill reference](https://github.com/multica-ai/andrej-karpathy-skills/tree/main/skills/karpathy-guidelines) | Retains the four Karpathy principles and adds suitable reuse, traceable state and flow, useful abstraction boundaries, domain names, rationale comments, and readable tests within approved scope |
| `doubt-driven-development` | Adapted | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills/tree/main/skills/doubt-driven-development) | Main-Agent orchestration that sends a minimal packet for fresh-context adversarial review, reconciles evidence, and bounds follow-up; no mandatory external CLI/model |
| `human-readable-communication` | Local adaptation | [danyuchn/asd-ste100-skill v0.4.0](https://github.com/danyuchn/asd-ste100-skill/blob/7d4a135a199a5d7447c4886bcd7ffe742a627bc9/SKILL.md) | Familiar words, clear actors, focused topics, stable terms, and conditions beside actions; rewritten as a relaxed, language-aware Main Agent presentation method that preserves meaning and harness contracts |
| `pr` | Synthesized | [agent-harness](https://github.com/jarupongbestt/agent-harness); Git/PR publication and evidence-based review practice | Original local synthesis for optional `open` and `ship`, preserving selected scope, current evidence, independent roles, and bounded recovery |
| `frontend-ui` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `api-design` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `database-migrations` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `data-pipelines` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `cloud-infrastructure` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |
| `observability` | Domain extension | Project-specific | Optional domain capability; no portable source is prescribed |

## PR synthesis

`pr` uses one original local definition, with required
[`references/open.md`](pr/references/open.md) and
[`references/ship.md`](pr/references/ship.md). It is not copied from Claude Ship
or claimed native to Claude Code, Codex, Pi, Hermes, or OpenCode. The
[first-party invocation references](../adapters/README.md#optional-pr-mapping),
accessed 2026-11-25, establish research anchors for native projection, not authorship,
source-code provenance, target discovery, or runtime validation. Each downstream
application still resolves the chosen local source's full commit SHA and access
date, reviews compatibility, inventories recursively linked assets, and rewrites
links into approved native destinations without copying the source tree.

## Code quality adaptation

`code-quality` replaces the local `karpathy-guidelines` name with one
[self-contained definition](code-quality/SKILL.md). It is a `local_adaptation`,
not an `upstream_direct` materialization or a second definition under an alias.
The historical attribution above remains: think before acting, choose the
simplest sufficient solution, keep edits surgical, and verify observable
outcomes. Proportionality and required behavior, tests, and safety controls
remain explicit.

Pinned influences, both accessed 2026-11-05:

- [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail/blob/96608e078b2d5ea607c5c3b8b5a54d1c1fba6846/skills/ponytail/SKILL.md),
  `skills/ponytail/SKILL.md`, commit
  `96608e078b2d5ea607c5c3b8b5a54d1c1fba6846`. Retain suitable project reuse,
  consideration of standard-library/platform features and installed dependencies,
  and complete changes that preserve required safeguards. Selection weighs
  behavior, compatibility, security, and maintenance cost. Omit first-match
  selection, bans on new dependencies, mandatory one-liners, persistent session
  modes, and upstream workflow instructions that would override harness roles
  or approved scope.
- [cursor/plugins pstack](https://github.com/cursor/plugins/blob/bb68f2365eceafa0cb2d253112f675acf5f144cc/pstack/skills/principle-minimize-reader-load/SKILL.md),
  `pstack/skills/principle-minimize-reader-load/SKILL.md`, commit
  `bb68f2365eceafa0cb2d253112f675acf5f144cc`. Retain traceable data flow,
  bounded mutable state, and abstractions that hide meaningful complexity or
  meet demonstrated requirements. Omit automatic inlining or removal based on
  caller count and the reader timing cap. Names, comments, and readable test
  structure are expressed as compact local guidance rather than mechanical
  size or timing rules.

The method applies only within approved scope. It does not replace TDD's
behavior and coverage method, root-cause investigation, independent review,
security work, or required checks. No upstream workflow, runtime, or executable
scaffolding is imported. Its registry default is the single local definition;
applications still resolve that local source's full commit SHA, access date,
compatibility, and assets before approval. The pinned influences are provenance,
not additional materialization sources or another direct candidate.

Supporting-asset decisions:

- [Ponytail `LICENSE`](https://github.com/DietrichGebert/ponytail/blob/96608e078b2d5ea607c5c3b8b5a54d1c1fba6846/LICENSE)
  and [pstack `LICENSE`](https://github.com/cursor/plugins/blob/bb68f2365eceafa0cb2d253112f675acf5f144cc/pstack/LICENSE):
  required and included together in [code-quality/LICENSE](code-quality/LICENSE).
  Both full, unmodified MIT notices are retained, including
  `Copyright (c) 2026 DietrichGebert` and `Copyright (c) 2026 Lauren Tan`.
- `pstack/skills/principle-guard-the-context-window/SKILL.md`: omitted; the
  adapted method is self-contained and the upstream context-window link is
  not retained.

## Human-readable communication adaptation

The upstream influence is `danyuchn/asd-ste100-skill`, `SKILL.md` v0.4.0,
resolved at commit `7d4a135a199a5d7447c4886bcd7ffe742a627bc9` and accessed
2026-10-02. The inspected source assets were `SKILL.md`,
`references/writing-rules.md`, `examples/before-after.md`, `scripts/ste-lint.py`,
and `LICENSE`. This is a `local_adaptation`, not a direct upstream copy or an
ASD-STE100 compliance claim. Its materialization default is the single
[local definition](human-readable-communication/SKILL.md); the three
direct candidates retain their policies. Applications still resolve the chosen
local source and supporting assets before approval.

The adaptation retains the clarity principles while removing an approved English
dictionary requirement, strict English grammar, and enforced word caps. English
20-word instruction and 25-word description targets are optional editing prompts.
An informal "80%" aim is not a measured score. Thai and mixed-language prose use
natural local phrasing, with longer sentences or passive voice where useful.
Meaning, evidence, uncertainty, and obligations take priority over brevity.

Supporting-asset decisions:

- `LICENSE`: required and included as
  [human-readable-communication/LICENSE](human-readable-communication/LICENSE),
  preserving the full MIT notice and `Copyright (c) 2026 Dustin Yuchen Teng`.
- `references/writing-rules.md`: omitted; its strict English summary is replaced
  by the self-contained, relaxed local method.
- `examples/before-after.md`: omitted; some examples change obligation or add
  content, which conflicts with meaning preservation.
- `scripts/ste-lint.py`: omitted; hard English word caps and punctuation checks
  are incompatible with the approved multilingual method. No replacement linter
  or automatic rewriting mechanism is introduced.

## Security and hardening adaptation

`security-and-hardening` uses one [local definition](security-and-hardening/SKILL.md)
classified as `local_adaptation`. Its historical template-harness and
secure-development influences remain in the table above. The additional influence
is [Cloudflare security-audit-skill](https://github.com/cloudflare/security-audit-skill/tree/d24bc269171a9171fac58493e0ffba511d571a4a),
commit `d24bc269171a9171fac58493e0ffba511d571a4a`, accessed 2026-11-25.
The inspected immutable source is not a claim about latest HEAD; mutable-main
views differed during source resolution.

The adaptation retains concrete boundary tracing, examination of existing
controls and plausible refutations, evidence-limited conclusions, minimal safe
validation, and the smallest effective correction. It adds local conditional
selection from ordinary scope or the actual diff, including indirect private
data paths. It distinguishes supported defects, exact evidence gaps, and optional
hardening within existing artifact contracts. Missing best practices alone do
not establish a vulnerability.

Compatibility choices preserve existing role ownership, approval, test handoffs,
retry policy, and independent Reviewer assessment. No nested audit agents,
six-phase orchestration, coverage ledger, new report schema or statuses, audit
output tree, validators, sandbox implementation, runtime integration, or permanent
Security Agent are imported. Existing approved tests keep their normal runtime
and permission controls; adversarial validation stays local, isolated, bounded,
and within verified permissions. An unsafe or unavailable decisive check leaves
an exact evidence gap and safe resolving check. No mandatory per-task screening,
field, checklist, scan, report, invocation, gate, or universal loading is added.
A full audit requires separately approved scope.

The local supporting assets are required and included:

- [references/boundary-cases.md](security-and-hardening/references/boundary-cases.md)
  is a short, selectively loaded set of examples; the entry skill owns the method.
- [LICENSE](security-and-hardening/LICENSE) preserves the complete, unmodified
  upstream MIT notice, including `Copyright (c) 2025-2026 Cloudflare, Inc.`.
  Downstream copies or substantial portions must retain the copyright and
  permission notice.

Source-asset inventory at the pinned commit follows. Paths without a root prefix
are under `skills/security-audit/`. Each disposition is intentional; links to
omitted companions and audit machinery are not projected into the local method.

| Source asset | Disposition and reason |
|---|---|
| [`SKILL.md`](https://raw.githubusercontent.com/cloudflare/security-audit-skill/d24bc269171a9171fac58493e0ffba511d571a4a/skills/security-audit/SKILL.md) | Translated selected core evidence and safe-validation principles into the local entry; audit setup and orchestration are outside conditional-guidance scope. |
| [`ATTACK-CLASSES.md`](https://raw.githubusercontent.com/cloudflare/security-audit-skill/d24bc269171a9171fac58493e0ffba511d571a4a/skills/security-audit/ATTACK-CLASSES.md) | Translated selected boundary prompts into the local reference; broad class coverage and agent assignments are outside scope. |
| [`DATA-ISOLATION-AND-LIFECYCLE.md`](https://raw.githubusercontent.com/cloudflare/security-audit-skill/d24bc269171a9171fac58493e0ffba511d571a4a/skills/security-audit/DATA-ISOLATION-AND-LIFECYCLE.md) | Translated selected private derived-data, cache, export, and lifecycle cases into the reference; domain-wide hunting is outside scope. |
| [`VALIDATION-AND-REPORTING.md`](https://raw.githubusercontent.com/cloudflare/security-audit-skill/d24bc269171a9171fac58493e0ffba511d571a4a/skills/security-audit/VALIDATION-AND-REPORTING.md) | Translated evidence and refutation reasoning into the entry; audit reporting and verifier orchestration are outside scope. |
| [Root `README.md`](https://raw.githubusercontent.com/cloudflare/security-audit-skill/d24bc269171a9171fac58493e0ffba511d571a4a/README.md) | Referenced for source scope and asset inventory; installer instructions are not copied or run. |
| [Root `LICENSE`](https://raw.githubusercontent.com/cloudflare/security-audit-skill/d24bc269171a9171fac58493e0ffba511d571a4a/LICENSE) | Included unmodified at the skill-local license path to preserve MIT obligations. |
| `RECONNAISSANCE.md` | Omitted; full-audit reconnaissance is outside conditional-guidance scope. |
| `HUNTING.md` | Omitted; hunting waves and coverage orchestration are outside conditional-guidance scope. |
| `AI-AND-LLM.md` | Omitted; the domain-wide audit catalog is outside the selected boundary examples. |
| `WEB-PROTOCOL-AND-AUTH.md` | Omitted; protocol-wide audit coverage is outside the selected boundary examples. |
| `CLIENT-SIDE.md` | Omitted; browser-wide audit coverage is outside the selected boundary examples. |
| `MEMORY-SAFETY-AND-BINARY.md` | Omitted; native and binary audit coverage is outside the selected boundary examples. |
| `SUPPLY-CHAIN-AND-RELEASE.md` | Omitted; release-wide audit coverage is outside the selected boundary examples. |
| `CLOUD-AND-DEPLOYMENT.md` | Omitted; deployment-wide audit coverage is outside the selected boundary examples. |
| `PROTOCOLS-RPC-AND-MESSAGING.md` | Omitted; transport-wide audit coverage is outside the selected boundary examples. |
| `RESOURCE-EXHAUSTION-AND-AVAILABILITY.md` | Omitted; availability audit coverage is outside the selected boundary examples. |
| `DESKTOP-MOBILE-AND-LOCAL-IPC.md` | Omitted; platform-wide audit coverage is outside the selected boundary examples. |
| `report-schema.json` | Omitted; a new audit-record contract is outside scope; existing harness artifacts remain authoritative. |
| `validate-findings.cjs` | Omitted; audit-report validation is outside scope and no report schema is imported. |
| `validate-findings.test.cjs` | Omitted; tests for the omitted audit validator are outside scope. |
| `validate-coverage-ledger.cjs` | Omitted; ledger validation is outside scope and no coverage ledger is imported. |
| `validate-coverage-ledger.test.cjs` | Omitted; tests for the omitted ledger validator are outside scope. |

Do not install a duplicate `security-audit` skill. The materialization default is
this single local adaptation, not the former addyosmani same-slug candidate or a
direct Cloudflare copy. Downstream applications resolve the actual selected local
adaptation's full commit SHA, access date, compatibility, and supporting assets
before approval, then rewrite links into approved destinations. The Cloudflare
pin records influence provenance; it is not the revision of future local copies.

## Provenance rules for future skills

Every new skill should add one row here with:

- an origin type
- a stable URL, file path, or explicit `project-specific` declaration
- a short description of what was adopted
- any meaningful rewrite or limitation

If a skill is derived from multiple sources, list all of them. If the sources
conflict, preserve the conflict for review instead of attributing a single source
to a blended rule.

## Documentation and ADRs local source

`documentation-and-adrs` materializes the single local
[definition](documentation-and-adrs/SKILL.md). Its historical influence remains
[knowledge-base](https://github.com/jarupongbestt/knowledge-base); this change does
not claim an upstream copy, invented pinned SHA, or new authorship. The local
adaptation supplies the harness's metadata, ADR lifecycle, human decision evidence,
and final-write integrity rules. The registry has no upstream candidate or local
fallback for this method. Applications resolve the actual selected local revision
and assets before approval. Include or translate the required ADR template and
knowledge contract through the approved native adapter map. Applicable final-write
checks remain mandatory, using a compatible existing target checker, translated
checks, or the optional Python 3.10+ standard-library reference checker. Only selection
of that reference checker requires its runtime and target layout/check command.
Illustrative ADR, log, and layout assets may remain references or be omitted with
reasons; inventory all dispositions and rewrite or remove projected links. Checker
maintainer tests are not automatically created, run, or copied into target projects.
