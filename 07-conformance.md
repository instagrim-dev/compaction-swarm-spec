# 07 — Conformance

## Profiles

Implementations **MUST** declare which **profile** they implement:

| Profile ID | Summary |
|------------|---------|
| `interactive-default` | Composite AND idle (**02**); default quorum (**04**); default shipping tier PR-only (**05**). |
| `headless` or `ci` | Human-idle omitted or substituted per **02**; other gates documented. |
| `human-checkpoint` | Optional; requires human batch approval before execution (**04**). |

Additional vendor-specific profiles **MUST** be named and versioned; they **MUST NOT** silently redefine `interactive-default`.

## Version string

Products **SHOULD** cite **`compaction-swarm-spec` / `MAJOR.MINOR`** (e.g. `0.2`) when claiming conformance. **0.x** drafts are unstable; **1.0+** signals stable normative clauses for the default profile.

## Required vs optional surfaces

**Required** for `interactive-default` claim:

- Composite eligibility as in **02** (with hysteresis **SHOULD**).
- Named **change_surface** and **merge_policy** with default PR-only / no auto-merge (**05**).
- Multi-agent quorum before expensive actions (**04**).
- Structured skip reasons for failed eligibility (**02**).

**Optional** but recommended:

- Headless/CI profile documentation.
- Artifact compaction semantics (**03**).
- Integration with external taxonomy docs (informative).

**Optional** and **not** required for conformance (operator-owned):

- Automated tests or CI jobs that **create real pull requests** on a live host (tokenized, disposable repo/fork, cleanup). Implementations **MAY** document such a workflow; the pattern does not require it for a valid `interactive-default` claim.

## Labels: optional export and optional research (07 index)

| Label | What it refers to |
|-------|-------------------|
| **Optional export** | Publishing the markdown spec as a **standalone public repository** ([spec README](./README.md#optional-standalone-public-repo)). |
| **Optional research** | A **per–work-class role matrix** beyond proposer+critic and k-of-n ([04](04-roles-and-quorum.md)). k-of-n is an allowed quorum form. See [Labels: optional export and optional research](./README.md#labels-optional-export-and-optional-research). |

## Conformance clause (informative)

An implementation **conforms** to profile **P** if it satisfies all **MUST** requirements in sections **02**, **04**, and **05** as specialized by **P** in this section, and documents known gaps.

## Operator vocabulary (BMO binding)

BMO emits **stable reason strings** for skips and gate outcomes so operators can grep logs, `/compaction`, and (when wired) traces. Treat these as **contracts** for observability; changing a string is a breaking change for dashboards.

| Area | Stable prefixes / codes | Defined in |
|------|-------------------------|------------|
| Composite idle gate | `compaction_gate_human_input`, `compaction_gate_bmo_run_active`, `compaction_gate_machine_load`, `compaction_gate_policy_window`, `compaction_gate_settling` | [ADR-003](https://github.com/instagrim-dev/bmo/blob/main/docs/decisions/003-idle-gates-compaction-spike.md); `internal/compaction/gate` |
| Quorum | `compaction_quorum_incomplete`, `compaction_quorum_rejected`, `compaction_quorum_threshold_not_met`, `compaction_quorum_timeout` | `internal/compaction/quorum` |
| PR / merge policy | `compaction_merge_blocked_protected`, `compaction_pr_idempotent_skip` | `internal/compaction/prpolicy` |
| Reactions preflight | `compaction_reaction_gate_unconfigured` (reaction uses `compaction_idle_gate` but `options.compaction.reaction_gate` is missing) | `internal/compaction/admission` |

Composition decisions (`compose.EnqueueDecision`) may attach **multiple** reasons; UIs SHOULD show the first or join with a stable separator.
