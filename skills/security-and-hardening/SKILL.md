---
name: security-and-hardening
description: Plan, test, implement, or review a concrete affected data, permission, or action boundary using source evidence and bounded validation.
---

# Security and Hardening

Use this method when ordinary task scope or the actual diff identifies a concrete
affected data, permission, or action boundary and operation. Include indirect paths
through private caches, derived copies, and exports. Task titles and keywords alone
neither trigger nor exclude security work.

Apply the method within the current role and approved scope. Selection adds no
mandatory per-task screening, field, checklist, scan, report, invocation, or gate,
and does not require every role to load this skill. Use the existing Plan, test
handoff, Task Result, and Review Result contracts. Independent assessment belongs
to the existing Reviewer stage; do not launch nested audit agents.

## Trace the affected boundary

1. **State the invariant.** Name the operation, authorized principal, protected
   resource or action, and intended restriction. Ground it in the task's acceptance
   criteria and existing policy or enforcing code. Include any promised revocation,
   deletion, or failure behavior that this operation affects.
2. **Follow a reachable path.** Trace the lower-trust actor or input through the
   intended control to the reachable crossing, affected principal or resource, and
   concrete impact. Follow relevant later consumers and derived copies; a safe
   primary write does not establish safe retrieval, export, or reuse. Cite the
   source locations and conditions supporting each consequential step.
3. **Try to refute the concern.** Inspect existing enforcing controls along that
   path, including identity binding, authorization, validation, normalization,
   framework behavior, containment, and lifecycle rules. Check whether another
   layer prevents the crossing, whether the proposed actor can reach it, and
   whether the result exceeds that actor's intended authority. Do not assume an
   external control is present or absent when source cannot establish it.
4. **Separate conclusions.** A supported defect needs a concrete violated invariant
   and evidence for the reachable impact. An evidence gap names the exact decisive
   fact still unknown and a safe check that can resolve it; it is not confirmation.
   Optional hardening is an improvement without an established boundary violation.
   Missing best practices, hypothetical chains, and intended same-principal behavior
   alone do not establish a vulnerability. Keep impact claims within the evidence.
5. **Correct the cause proportionately.** Recommend or implement, as the role and
   approved scope permit, the smallest effective correction at the enforcing
   decision point. Preserve legitimate behavior and derive relevant regression
   coverage from acceptance criteria. Reuse sufficient existing coverage; author
   security tests only where it is insufficient. Preserve the approved same-slice
   Test Engineer handoff before Builder work. `test_subtask.action: none` adds no
   Test Engineer stage.

Consult only the relevant section of
[boundary cases](references/boundary-cases.md) when the affected operation needs
an example. The reference supplies prompts, not another mandatory checklist.

## Validate safely

Prefer source inspection and existing approved checks with dummy principals,
non-sensitive fixtures, and dummy secrets. Existing approved tests retain their
ordinary runtime and permission controls; this method imports no audit runtime
or sandbox implementation.

Adversarial validation must be local, isolated, bounded, and within verified
permissions. Establish the minimum effect needed to resolve the boundary question,
then stop. Protect secrets in prompts, logs, commands, and retained evidence;
capture only the non-secret inputs and results needed to explain the conclusion.
Do not probe production or shared endpoints, use real credentials or private
data, spend paid quota, or extend a check into persistence or a larger attack.
If a decisive check is unsafe or unavailable, retain the exact evidence gap and
safe local or owner-observed resolving check rather than execute it or claim
confirmation.

## Stop at the scoped operation

Stop when the affected path and its relevant consumers have an evidenced control,
a bounded correction with appropriate coverage, or an explicit unresolved fact.
Report remaining uncertainty through the current role's artifact. Expand only
when concrete evidence connects another path to this operation; follow existing
scope and approval rules for material changes. A full audit needs its own scope
and approval. This method makes no whole-system coverage claim and introduces no
permanent Security Agent, audit phases, coverage ledger, schema, validator, or
audit output tree.

This is one local adaptation influenced by
[Cloudflare's pinned security-audit skill](https://github.com/cloudflare/security-audit-skill/tree/d24bc269171a9171fac58493e0ffba511d571a4a).
See [provenance and asset decisions](../origins.md#security-and-hardening-adaptation).
The complete upstream MIT notice is retained in [LICENSE](LICENSE); preserve it
when distributing copies or substantial portions of this adaptation.
