# Open or reuse a PR

Use this procedure for `open`, and for the publication portion of `ship`.
The [canonical policy](../../../SPEC.md#optional-pr-capability) governs authority;
the [PR Operation Result](../../../schemas/artifacts.md#pr-operation-result)
defines the checkpoint and outcome. Do not copy the full schema into this skill
or invent a storage destination.

## Resolve identity and state

Start or resume the same operation record. Preserve its original authorization,
history, and completed/partial mutations. Record finite observation/wait and
infrastructure-rerun limits at start; `open` does not start CI observation and may
record those budgets as zero/inapplicable with a reason. An unavailable operation
history that prevents safe reconciliation is a blocker, not a fresh attempt.

Inspect the repository's instructions, PR template, conventions, current branch,
index, worktree, untracked files, and existing local/remote history. Discover the
host, usable account, remotes, and adapter capabilities for exact PR lookup,
comparison, commit, push, creation, and post-publication inspection. Resolve and
record all of:

- host and base repository identity plus target branch;
- full head repository identity plus source branch;
- fork relationship, or explicit same-repository identity;
- current base/head SHAs and the local branch's relationship to remote history.

Do not assume `origin`, the default branch, current account, or branch name alone
identifies the intended destination. Ask for the necessary selection when remotes,
accounts, forks, or targets conflict. Missing capabilities or permissions block
their mutations. Do not auto-fork or switch repositories to bypass a mismatch.
On a detached HEAD, target branch, or otherwise unsafe write branch, resolve the
user-selected safe branch/isolation method before writing; do not publish from
that state by assumption.

Look up PRs by the exact host, base repository/branch, **head repository and
branch**. Inspect state explicitly. A same-named branch in another fork is not a
match. Multiple plausible matches require resolution. A closed or merged match
does not authorize reopening or repurposing it; report its URL/state and obtain
the necessary next-action decision. Do not silently create a replacement.

## Select the entire resultant change

Audit the prospective **entire actual PR diff against the resolved target**,
using the host's comparison semantics and recorded base/head revisions, including
the relevant common ancestor. Inspect pre-existing branch commits as well as
candidate staged, unstaged, and untracked local work. A Run Summary can supply
provenance; it cannot authorize files, limit the audit to one run, or replace the
actual diff. Selected untracked content needs the same inspection as tracked work.

Identify topics in the complete change. Propose grouping related work and
separating unrelated work; resolve the user's chosen set and record selected and
excluded topics, exact local changes/hunks, and permitted mutations. An explicit
request already choosing a clear coherent set needs no repeated approval. Do not
silently include unrelated branch history or local work, or create multiple
branches/PRs. If an existing branch mixes topics, resolve a safe isolation method
and any additional authority before proceeding. Scope must describe the complete
resulting PR, including changes already on an existing open PR.

Record an inspectable reference for the resultant full diff and selected local
snapshot, plus enough baseline evidence to preserve unrelated state. Preserve
unselected staged and unstaged hunks even when they share a file with selected
work. Use a suitably isolated index/worktree or another verified selective method
when available and authorized. Never use blind `git add -A` or commit the whole
existing index. Do not reset, amend, rebase, force-push, overwrite user state, or
silently stash/discard changes to manufacture isolation. If the selected set
cannot be safely isolated and audited, stop before mutation with the precise
decision or capability needed.

Immediately before each mutation, recheck relevant local state, base/head refs,
remote branch, and PR identity against the checkpoint. Concurrent changes require
fresh comparison and scope reconciliation; do not overwrite them or treat stale
selection as authority. If they materially change the selected publication, ask
only for that changed decision.

## Publish the selected set

Use the resolved explicit action as authority for the recorded, scoped commit,
normal push, and PR creation/update that are needed. Respect host permission
gates; avoid asking again merely because publication mutates state. Additional
branch/isolation authority or scope changes still need their missing selection.

Commit only selected new local work, when present. Inspect hook/tool side effects
and the actual committed diff; verify it still matches the selected set and
preserves unrelated index/worktree state. Unexpected generated changes or hooks
must not slip into publication. Reconcile them before pushing; stop if a safe
correction would require unapproved history rewriting or user-state changes.
Push only to the resolved head repository/branch without rewriting remote history.
Verify the pushed SHA and full comparison against the exact target still match
the selection, including after any tool side effects.

If the exact open PR exists, reuse it. Publish selected new work to its head and
return that same PR's URL; if no new work needs publishing, avoid an unnecessary
commit or push. An empty diff with no open PR yields an explicit no-diff/no-PR
outcome, not an empty commit or fabricated success. For an existing open PR whose
actual diff has become empty, report that state and URL without claiming a
completed selected publication or silently closing it.

For a new PR, derive a concise accurate title/body from the **complete selected
resultant change**. Follow repository templates and existing conventions; describe
behavior, useful validation and limitations, and available source references.
Do not present old or unrun checks as current passes. Inspect existing PR text
before updates; do not silently overwrite human-edited content. If a necessary
description correction conflicts with human edits, present the proposed change
for the necessary decision. Creating the PR is authorized; reviewers, labels,
comments, other notifications, and unrelated metadata changes require separate
authority.

Use structured tool arguments or a body file for multiline text, with safe
argument handling rather than shell interpolation of descriptions or other
external text. Do not treat a tool's nominal preview as read-only without
verification: `gh pr create --dry-run`, for example, can push. No particular CLI
or shell permission is a prerequisite imposed by this skill.

## Reconcile and finish

After any failure or timeout, inspect actual local commits, remote refs, and PRs
before retrying. Record known completed effects and unresolved partial effects
with their SHAs/URLs; never repeat an uncertain commit, push, or create blindly.
Resume the original operation/checkpoint, within recorded finite limits, once
state and authority are reconciled. Unknown, transient, permission, and platform
failures do not authorize code repair or new publication scope.

Read the actual published PR contents and fresh base/head identity and SHAs from
the host. Confirm the full diff matches the selected scope, the head is the
intended pushed revision, and the PR is open against the exact target. Refresh
comparison when base or head moved; unresolved concurrent changes prevent a
completion claim. Record mutation evidence, preserved unrelated state, explicit
outcome, blockers/limitations, and the next permitted action under the contract.

For `open`, return the exact PR URL and outcome, then **stop**. Do not implicitly
start independent review, wait for or repair CI, merge, or deploy. Leave review
and CI fields explicitly inapplicable to this action rather than inventing
evidence. For the publication portion of `ship`, hand the reconciled identity,
revision, scope, history, and checkpoint back to its procedure.
