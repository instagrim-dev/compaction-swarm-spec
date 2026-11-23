# 01 — Taxonomy and placement

## Position relative to common swarm patterns

Implementations MAY map this pattern to existing taxonomies (e.g. hive, colony, flock, pack, swarm, ecosystem). **Compaction swarm** is characterized by:

- **Activation:** Primarily **idle-gated** (resource and human-attention slack), not only calendar time or continuous streaming.
- **Coordination:** **Environment-mediated** (artifact-mediated)—agents converge on shared durable state (files, tickets, registries) rather than requiring a single orchestrator message for every step.
- **Outcome:** **Compaction**—reducing duplicate hypotheses, stale documentation, and scattered debt notes into a smaller set of actionable artifacts.

## Relationship to meta-orchestrator patterns

An implementation MAY combine compaction swarm with an outer **classifier / shaper / monitor** (adaptive orchestration). If so:

- **MUST NOT** allow two automated subsystems to fight for the same **exclusive** resource (e.g. concurrent promotion to protected branches, or concurrent unbounded child spawn) without **documented arbitration** (see **05** and product-specific monitor boundaries).

## Distinctive comparisons

| Pattern | Typical trigger | Compaction swarm differs by… |
|---------|-----------------|------------------------------|
| Interactive pair-programming | User message | Idle eligibility + compounding artifacts + default PR path |
| Cron / scheduled job | Wall clock | Composite gates + quorum + artifact convergence |
| CI-only automation | Push / PR event | Optional profile; default assumes **human-idle** dimension where applicable |
| Always-on service agent | Process uptime | Explicit idle and policy dimensions |

## Normative alignment (swarm-orchestration-spec)

Implementations claiming interoperability with the **[swarm-orchestration-spec](https://github.com/instagrim-dev/swarm-orchestration-spec)** (cite **v1.1** or later) vocabulary SHOULD describe **idle-gated compaction swarm** as:

- A **colony- or flock-scale** pattern: multiple workers with differentiated roles, coordinated through **shared durable artifacts** (environment-mediated coordination) rather than a single chat thread.
- **Activated by idle and resource slack** (human-attention and machine/policy dimensions in **interactive** profile; **headless/CI** profile substitutes the human-idle leg per **02**), not solely by wall-clock schedule or push events—though a host MAY use a schedule as a *candidate* trigger that still passes through the composite gate.
- **Outcome-oriented toward compaction** (fewer, higher-trust artifacts) rather than open-ended exploration; contrast with “always-on” service agents or one-shot parameterized tasks.

This spec does not redefine terms from swarm-orchestration-spec; it **extends** that taxonomy with **gate profiles**, **quorum**, and **PR-only default tier** (see **04–05**, **07**). Where metaphors conflict, **07** conformance labels win.

## Primitives binding (optional)

When compaction uses **signal deposits** or **workspace claims** (not only file-based artifacts), implementations SHOULD use the [Agent Coordination Substrate](https://github.com/instagrim-dev/agent-coordination-substrate) advisory and enforcement layers, or provide equivalent mortality, zone scope, and operator-override semantics.

## Document status

Taxonomy naming is **informative**; conformance is defined in **07** by **profile** and **capability**, not by ecosystem metaphor alone.
