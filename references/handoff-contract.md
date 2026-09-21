# Handoff contract

Use headings that make the following content easy to verify. Equivalent local
formats are acceptable when the owning repository already defines a task record.

## Identity and objective

- Old coordinator identity and affected repository or project.
- Current objective, priority, and completion criteria.
- Owning Issue, PR, Project, or equivalent tracker.

## Authority and decisions

- Applicable instructions, canonical policies, owning contracts, revisions,
  commits, or URLs.
- Accepted decisions with their sources.
- Explicitly superseded or rejected directions.
- Approvals already consumed and approvals still available, each with its exact
  scope. Do not transfer authority by implication.

## Current observed state

- Branch, HEAD, staged, unstaged and untracked state for each affected checkout.
- Remote synchronization and PR/review/CI state.
- Tracker status and remaining completion criteria.
- Completed work, changed artifacts, actual checks and unavailable checks.
- External effects and readback. Mark uncertain outcomes without suggesting an
  unconditional replay.

## Open state

- Hypotheses, unresolved decisions, blockers, prohibited actions, and known
  conflicts between evidence and authority.
- Recovery route and facts showing what was not mutated.
- Protected evidence location and required access, without secret contents.

## Resumption

- Checks the successor performs before accepting the handoff.
- One next action after acceptance.
- Routing record: coordinator identity, selected model and effort when the host
  supports them, selection reason, completion condition, transient client ID if
  any, resolved task ID, tracking method, and acknowledgement.

## Completion states

`handoff prepared` means the checkpoint exists and has been verified against
available evidence. `successor dispatched` additionally requires a resolved
task identity. `rollover accepted` additionally requires the successor's
`accepted` acknowledgement. These states must not be collapsed.
