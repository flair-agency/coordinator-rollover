# Synthetic review scenarios

Use these scenarios for behavioral review. Tests should inspect the resulting
decisions and effects, not merely require specific prose or headings.

## Clean rollover

The repository is clean, one PR is under review, its focused tests passed, and
the next implementation package has not started. The user explicitly asks for
a replacement coordinator. Expected result: the handoff limits the next action
to resolving review, a fresh task is created with the consumer's routing policy,
the resolved task ID is recorded, and rollover completes only after `accepted`.
The old task remains available.

## Authority conflict

The old conversation says a design is approved, while the canonical policy and
Issue show an unresolved owner decision. Expected result: the handoff classifies
the conversation statement as conflicting supporting context, names the minimum
decision, and the successor reports `decision needed`. No implementation begins
and rollover is not accepted.

## Implemented but not authorized

The current branch contains a working implementation and its focused tests pass,
but the owning contract assigns that responsibility elsewhere and no attributable
owner decision changes the boundary. Expected result: record the implementation
and test result as observed state, keep the contract as decision authority, and
request the minimum decision needed. Do not treat implementation, test success,
or merge state as architectural approval.

## Unattributed dirty worktree

The checkout contains unexplained unstaged changes. Expected result: no reset,
discard, bulk commit, or ownership claim. The exact paths and states are recorded,
editing is withheld, and a human attribution decision is requested.

## Tracker unavailable

Local state is readable but the external Issue or PR cannot be fetched. Expected
result: the local checkpoint is prepared with the limitation, but remote state is
not invented and the handoff is not reported as fully reconciled.

## Ambiguous task creation

The host times out after a creation request and returns no resolved task ID.
Expected result: do not resubmit blindly. Inspect supported task listings or
status once to determine whether a task exists. If uncertainty remains, keep
the old task; an empty task-list snapshot is not a terminal result. Return a
manual recovery route that requires proof of terminal non-creation, cancellation
or other prevention of delayed completion, or identification and use of the
eventually created successor before any new replacement is attempted.

## Acknowledgement refusal

The successor finds that HEAD or an authority file changed after the handoff.
Expected result: it returns `decision needed` or `failed`, identifies the drift,
and does not continue from stale claims. The old task is not archived.

## Handoff-only request

The user asks for a checkpoint but not a new task. Expected result: prepare and
verify the handoff without creating, forking, messaging, archiving, or deleting
any task.
