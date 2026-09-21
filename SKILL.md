---
name: coordinator-rollover
description: Create an evidence-backed handoff for a long-running Codex coordinator and, only when explicitly requested, start and verify a replacement task. Use when the user asks to roll over, replace, retire, refresh, or hand off a coordinator; do not use it to infer degradation automatically.
---

# Coordinator Rollover

Replace a coordinator without treating its conversation memory as authority.
The safe outcome is a compact, verifiable handoff; a new task is a separate
effect that requires an explicit user request.

## Choose the requested outcome

- If the user asks only for a handoff or checkpoint, create the handoff and
  stop. Do not create, fork, archive, or delete a task.
- If the user explicitly asks to replace, roll over, or hand off to a new
  coordinator, create a new task only after the handoff is ready and the host
  supports user-visible task creation.
- Do not infer a request to create a task from concern about context quality.
  Do not score, monitor, or diagnose degradation in this version.

## Establish the rollover target

Identify the current objective, old coordinator task, affected repositories,
owning Issue or equivalent tracker, and completion criteria. Resolve ambiguous
targets before creating a successor. A missing task ID is recordable; an
ambiguous repository or objective is a stopping condition.

Follow every applicable repository instruction and tracking policy. Inspect
each affected worktree before mutation and preserve unrelated changes and the
staged/unstaged distinction.

## Reconstruct current state

Reconstruct three distinct kinds of information:

- **Observed state:** repository and worktree state; PR, review, CI, and test
  results; tracker readback; release and operation evidence; and external
  effects with readback.
- **Decision authority:** adopted canonical policies, owning contracts,
  applicable instructions, and explicit owner decisions whose source can be
  identified.
- **Supporting context:** the handoff produced and verified in this run, the
  old conversation, and the coordinator's own claims.

Observed state tells you what is. Decision authority tells you what is allowed.
Supporting context helps locate and interpret evidence but cannot establish an
accepted decision by itself. Existing code may be an unapproved proposal, and
a merge does not prove production behavior. When sources conflict, record the
observed state, applicable authority, and minimum decision needed. Do not
silently reconcile the conflict or promote a conversation claim into an
accepted decision.

Classify every material statement as an accepted decision, observed fact,
hypothesis, or unresolved item. Verify current Git branch, HEAD, staged,
unstaged and untracked changes, remote synchronization, PR state, tracker state,
and the exact checks that actually ran. Preserve unavailable evidence as an
explicit limitation.

## Write the handoff

Use the contract in [references/handoff-contract.md](references/handoff-contract.md).
Keep the handoff compact and link to detailed history rather than copying it.
Do not include raw conversation history, secrets, credentials, production
exports, unnecessary successful logs, or obsolete plans. Point an authorized
operator to protected evidence without copying it.

The successor's first action must be to compare the handoff with repository,
authority, and tracker state. Give it one next action after that comparison,
not a speculative queue.

## Create and verify a successor only when requested

Use the host's supported task-creation function rather than a subagent or a
history fork. A full-history fork is not the default because it reimports the
context the rollover is meant to replace.

Apply the consumer repository's task-routing policy. Record the selected model,
reasoning effort, reason, completion condition, and old coordinator identity.
Do not claim a model or effort was selected if the host cannot set or expose it.

Send only the compact handoff, required canonical sources, affected checkout,
and acceptance request. If creation first returns a transient client ID, obtain
the resolved task ID before treating dispatch as complete. Ask the successor to
return exactly one outcome:

- `accepted`: state and authority agree, and the successor can continue;
- `decision needed`: a named conflict or missing authority requires a human;
- `failed`: the handoff cannot be verified or the task cannot proceed.

Wait with the supported bounded or event-driven task-management function. Do
not repeatedly poll unchanged state. Do not retry an uncertain creation request
blindly; first resolve whether a task already exists.

Never archive or delete the old task automatically. Successor acknowledgement
does not itself authorize archival.

## Stop safely

Stop successor creation or completion claims when:

- canonical authority conflicts and precedence does not resolve it;
- repository, objective, task, or tracker identity may refer to the wrong work;
- an external write outcome is uncertain and readback is missing;
- worktree changes cannot be attributed without risking another owner's work;
- the user did not explicitly request a new task;
- the host lacks supported task creation or resolved identity; or
- successor acknowledgement is missing, `decision needed`, or `failed`.

Return the completed handoff, exact missing evidence or decision, effects that
already occurred, and the manual continuation route. Do not describe handoff
completion as rollover completion when no successor was acknowledged.

## Verify and report

Read [references/scenarios.md](references/scenarios.md) when reviewing,
testing, or changing this Skill. Report the handoff location or content, old and
new resolved task identities when applicable, acknowledgement, repository and
tracker state, actual checks, limits, remaining human decisions, and recovery.

The manual route is available when creation state is known: a human creates a
fresh task, supplies the handoff and canonical references, records the resolved
task ID, and obtains the same acknowledgement. If an earlier creation request
has an uncertain outcome, absence from a task-list snapshot is not enough to
permit another attempt. The human must first prove that the request reached a
terminal non-creation state, cancel or otherwise make it unable to produce a
task, or identify and use the successor it eventually created. Until one of
those outcomes is verified, do not create another task manually or
automatically. Automation is not evidence that the handoff is correct.
