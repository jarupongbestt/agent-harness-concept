# Boundary Cases

Load only the section relevant to the affected operation. These examples help
apply the [entry method](../SKILL.md); they do not trigger work by keyword or
require a scan of every category.

## Identity, permissions, and delegated actions

- A bulk operation authorizes its parent request but accepts object IDs belonging
  to another owner. Compare the effective permission for each affected item.
- A delegated action uses a broader service identity. Check that the requested
  resource and operation remain bound to the caller's authority, including later
  execution after a role change.

## Private data, derived copies, caches, and exports

- A cache optimization changes keys for private results. Follow ownership through
  cache insertion and retrieval; a tenant field on the primary record alone does
  not prevent a shared-key disclosure.
- A permission change hides the primary record while a preview, search result,
  generated copy, or export still serves it. Check the relevant retrieval and
  invalidation rules against the promised access boundary.
- An export selects permitted records but adds restricted fields or leaves the
  finished artifact downloadable by another principal. Inspect selection and
  artifact access. Deletion or restore matters where an explicit lifecycle
  guarantee covers that copy; preference differences alone are not defects.

## Untrusted input to execution, files, queries, and networks

- A stored value becomes a command argument, query fragment, or path in a later
  consumer. Check the control at that consumer, including normalization and file
  resolution, rather than relying on validation for the original storage context.
- A permitted URL redirects to a forbidden destination. Trace the actual network
  operation and existing destination controls without probing a shared endpoint.

## Secrets, sensitive state transitions, and failure behavior

- A failure path includes a credential in diagnostics accessible to a different
  reader. Establish the secret's data class and that reader's actual access.
- Revocation, recovery, rollback, or a failed authorization check leaves queued
  work or restored state able to perform a now-forbidden operation. Inspect the
  current authority at the consequential transition and the intended failure rule.

Stop at the affected operation and concrete downstream consumers. Use dummy
principals and fixtures under the entry method's validation limits; an external
policy or runtime fact that cannot be checked safely remains an exact evidence
gap. Broader hunting needs separately approved scope.
