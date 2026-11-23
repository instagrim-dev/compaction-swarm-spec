# Idle-gated compaction swarm (draft spec)

**Status:** Draft **0.2** (spec-normative). Public home: [compaction-swarm-spec](https://github.com/instagrim-dev/compaction-swarm-spec) tag **`v0.2.0`**. Conformance cite: **`compaction-swarm-spec` / `0.2`**.
**Implementation status:** the BMO binding is **shipped product behavior** (`internal/compaction/*`, recipe ref `builtin://compaction`), not a future seam — see the [compaction topic](https://github.com/instagrim-dev/bmo/blob/main/docs/topics/agent/compaction.md). The spec **draft version** tracks *normative-clause* stability ([§07 versioning](07-conformance.md#version-string)) and is independent of implementation maturity.
**Normative files:** `00`–`07` in this directory.
**Product planning:** maintainer artifact outside this repository (workspace-root, maintainer-internal).
**Requirements origin:** maintainer artifact outside this repository (workspace-root, maintainer-internal).
**Idle gate spike:** [ADR-003](https://github.com/instagrim-dev/bmo/blob/main/docs/decisions/003-idle-gates-compaction-spike.md) — signal inventory, profiles, reason codes (informative for implementers).
**Orchestration seam:** [ADR-004](https://github.com/instagrim-dev/bmo/blob/main/docs/decisions/004-compaction-orchestration-seam.md); Go package `internal/compaction/gate`.
**Quorum:** Go package `internal/compaction/quorum` (two-of-two and k-of-n threshold quorum, timeout → deny); spec §04.
**PR policy:** Go package `internal/compaction/prpolicy` (no auto-merge to protected default tier, branch naming, process deduper); spec §05.
**Composition tests:** `internal/compaction/compose` — gate + deduper preflight (`EnqueueDecision`) and scenario tests for the five plan integration targets (without live scheduler/`gh`).
**Admission runtime:** Go package `internal/compaction/admission` — composes gate + deduper at runtime, performs reaction-gate preflight (`compaction_reaction_gate_unconfigured`), and records bounded per-session telemetry for `/compaction` surfaces.
**Built-in executor:** Go package `internal/compaction/executor` — backs the reserved `builtin://compaction` recipe ref with a repo-owned prompt contract, so scheduler jobs and reactions can run self-directed stewardship without a hand-authored YAML recipe (ADR-004 §2–3).

> **Scope note — two senses of "compaction":** this spec governs **idle-gated repository stewardship** (the swarm). The separate Go package `internal/compaction/streaming` implements **session compaction** (context-window recovery: mechanical → semantic hot-swap, degraded fallback) — a distinct concept that shares only the package namespace. It is **out of scope** for this spec; see [session-compaction.md](https://github.com/instagrim-dev/bmo/blob/main/docs/topics/agent/session-compaction.md) and the [compaction topic boundary](https://github.com/instagrim-dev/bmo/blob/main/docs/topics/agent/compaction.md#boundary).

**Related specs (external):**

| Spec | Role |
|------|------|
| [swarm-orchestration-spec](https://github.com/instagrim-dev/swarm-orchestration-spec) v1.1 | Orchestration patterns — taxonomy, meta-orchestrator, environment-mediated coordination |
| [agent-coordination-substrate](https://github.com/instagrim-dev/agent-coordination-substrate) | Primitives — signals, zone pressure, mortal claims (optional binding for artifact/stigmergy layers) |

## Labels: optional export and optional research

Use these names in plans, issues, and cross-repo discussions so **optional** work is not mistaken for **blocking** scope (draft **0.2**).

| Topic | Label | Meaning |
|-------|--------|---------|
| Standalone public git repo for this spec only | **Optional export** | Published snapshot: [compaction-swarm-spec](https://github.com/instagrim-dev/compaction-swarm-spec) tag `v0.2.0`. This directory in BMO remains the working source for the exporter under [Optional: standalone public repo](#optional-standalone-public-repo). Not required for draft **0.2** review or BMO implementation. |
| Role matrix per work class | **Optional research** | Normative text in **04** allows proposer+critic **or** k-of-n, and BMO ships both through `internal/compaction/quorum`. Different role bundles by work class (docs vs. code vs. large refactor) remain out of scope for **0.2** unless a product adopts them — see maintainer artifact outside this repository. |

## Contents

| Doc | Description |
|-----|-------------|
| [00-overview.md](00-overview.md) | Scope, goals, reading order |
| [01-taxonomy-and-placement.md](01-taxonomy-and-placement.md) | Pattern placement vs hive/colony/flock/pack/swarm/ecosystem |
| [02-idle-and-resource-gates.md](02-idle-and-resource-gates.md) | Composite idle, profiles, headless/CI |
| [03-artifacts-and-compounding.md](03-artifacts-and-compounding.md) | Shared artifacts, dedupe, convergence |
| [04-roles-and-quorum.md](04-roles-and-quorum.md) | Agents, quorum, timeouts, escalation |
| [05-safety-and-permissions.md](05-safety-and-permissions.md) | Conformance matrix, default PR-only |
| [06-failure-and-recovery.md](06-failure-and-recovery.md) | Partial success, stuck swarms, abandoned work |
| [07-conformance.md](07-conformance.md) | Profiles, versioning, optional surfaces |

## Versioning

**0.x** — editorial and additive changes. **1.0** — first stability promise for normative clauses. Breaking normative changes bump major version.

## Optional: standalone public repo

The public repository for these markdown files is [instagrim-dev/compaction-swarm-spec](https://github.com/instagrim-dev/compaction-swarm-spec), tag **`v0.2.0`** (draft **0.2**). Conformance claims cite **`compaction-swarm-spec` / `0.2`** ([07-conformance.md](07-conformance.md#version-string)).

This directory in BMO remains the working source. Republish with the mechanical exporter in **[`scripts/compaction-spec-export`](https://github.com/instagrim-dev/bmo/tree/main/scripts/compaction-spec-export/README.md)** (command: `OUT=/path/to/dir task compaction-spec:export` from the BMO module root). It is **not** run in default CI. The exporter copies `*.md`, rewrites cross-links to BMO on GitHub, and adds **LICENSE** (CC-BY-4.0) plus **EXPORT.md** provenance. Review the diff, push, and tag the draft version on the status line (**`v0.2.0`** for draft **0.2**). Then run normative review ([07-conformance.md](07-conformance.md), MUST/SHOULD, [swarm-orchestration-spec](https://github.com/instagrim-dev/swarm-orchestration-spec) v1.1, [agent-coordination-substrate](https://github.com/instagrim-dev/agent-coordination-substrate) where primitives apply).

The exporter README's example tag is **`v0.1.0`**, and `export.sh` still stamps cite **0.1** into **EXPORT.md**. Those generator strings are unchanged. The normative version is the status line (**0.2**).

## Optional: live PR end-to-end (operators)

Verifying **`gh pr create`** or host APIs against a **real** repository is **optional**. BMO’s shipped validation includes **`httptest`** (REST list-by-head), opt-in **`gh pr create --dry-run`** (`task test:compaction:gh`), and an opt-in disposable-repo real PR lane (`task test:compaction:gh:real`) that requires `BMO_COMPACTION_GH_REAL_REPO` plus GitHub auth and teardown. Do not treat that lane as a default gate for the BMO binary.
