# 04 — Roles and quorum

## Roles (informative)

Typical roles include:

- **Proposer** — Identifies work, scope, and expected value.
- **Critic / reviewer** — Challenges assumptions, risk, and test/rollback posture.
- **Executor** — Applies tool actions within allowed **change surface** (**05**).

Implementations **MAY** collapse or combine roles on small tasks; **MUST** still satisfy quorum rules for the conformance profile.

## Default quorum (normative for default profile)

Before **PR-sized or otherwise expensive** actions (implementation-defined list per product), the default profile **MUST** require **multi-agent quorum**: e.g. proposer + critic agreement, or **k-of-n** approval on value and scope.

Quorum **MUST** be implemented as **protocol**, not as unstructured multi-agent chat:

- **Explicit roles** or voters.
- **Timeouts**; if quorum is not reached, default **deny** expensive action **OR** documented escalation.
- **Early completion** when required votes are satisfied.

## Human checkpoint (optional profile)

A conformance profile **MAY** require **human batch approval** before execution. This **MUST** be named explicitly in **07**; it is **not** the default profile.

## Merge and promotion

In the **default shipping tier** (**05**), humans **MUST** retain merge/promotion authority to protected branches; quorum among agents does **not** substitute for that gate unless a **separate, named** profile says otherwise.

## Label: optional research (role matrix / richer bundles)

- **Optional research — role matrix per work class:** This section **MUST** require multi-agent quorum for the default profile; it **MAY** be satisfied by proposer+critic **or** k-of-n. **BMO** now implements both the default **two-role** path and threshold **k-of-n** in [`internal/compaction/quorum`](https://github.com/instagrim-dev/bmo/blob/main/internal/compaction/quorum/doc.go), and the built-in executor uses that quorum before commit or PR promotion. Defining different role bundles by work class remains **optional research** (see [Labels: optional export and optional research](./README.md#labels-optional-export-and-optional-research) and the compaction plan maintainer artifact outside this repository).

| Spec §04 allowance | BMO `internal/compaction/quorum` today |
|--------------------|----------------------------------------|
| Proposer + critic agreement | **Yes** — default pattern; proposer + critic both approve before promotion. |
| k-of-n approval (k < n) | **Yes** — threshold approvals are supported through `RequiredApprovals`. |
| Different roles or matrices per work class (docs vs. code vs. large refactor) | **No** — canonical roles are `proposer` and `critic`; callers may add extra labels, and there is no registry by work class. |
| Executor as a voter | **No** — executor is informative in this spec; not a [Role] in code. |
