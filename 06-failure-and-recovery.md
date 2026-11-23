# 06 — Failure and recovery

## Partial success

Implementations **SHOULD** define behavior when:

- Branch push succeeds but **PR creation fails**.
- PR exists but **CI** fails repeatedly.
- **Quorum** times out mid-flight.
- Human **force-pushes** or rewrites history under the agent branch.

Recovery **SHOULD** favor **idempotent** retries keyed by logical work item, not blind duplication of PRs.

## Stuck swarm

If agents deadlock or loop, implementations **SHOULD**:

- Apply **wall-clock limits** and **resource caps**.
- Surface **operator-visible** status (stuck reason, last artifact touch).
- Offer **manual cancel** or **snooze** without editing raw config where possible.

## Abandoned work

Long-lived branches or drafts **SHOULD** be **garbage-collected** or marked abandoned per policy, with audit preserved if required.

## Starvation

If **policy-ok** never becomes true (e.g. CI always red, zero budget), implementations **SHOULD** emit **starvation** signals in observability so operators can adjust policy rather than silently waiting forever.
