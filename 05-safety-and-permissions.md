# 05 — Safety and permissions

## Conformance matrix

Implementations **MUST** expose **named** conformance controls on at least:

- **change_surface** — What agents may mutate (e.g. artifacts-only, local branch, remote branch + PR).
- **merge_policy** — Whether automatic merge to protected branches is allowed (default: **no**).

Implementations **SHOULD NOT** rely on ambiguous numeric “levels” without definitions. **Narrower** surfaces (more restrictive) and **broader** surfaces **MUST** be separately named.

## Default shipping tier (normative)

The **default shipping tier** for products claiming this spec’s default profile **MUST**:

- Allow agents to create or update **branches** and **pull requests** (or host-equivalent review objects) where the host supports it.
- **NOT** automatically merge to **protected** or **default** production branches.

## Deterministic gates

Implementations **SHOULD** wrap probabilistic agent output with **deterministic** checks: linters, tests, security scanners, size/complexity limits, dependency policy, and required CI status—so omissions are caught structurally.

## Secrets and destructive operations

Implementations **MUST** apply **least privilege** to credentials used for compaction. High-risk tools (secrets, production, destructive git operations) **SHOULD** require stricter profiles or human gates.

## Arbitration with other automations

If multiple automations (e.g. adaptive monitor, health reactions, compaction) can affect the same session or repo, implementations **MUST** document **arbitration** so two subsystems do not concurrently violate caps or branch policy (see product-specific monitor boundary docs where present).
