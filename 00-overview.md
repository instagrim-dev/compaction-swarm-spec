# 00 — Overview

## Purpose

This specification defines **idle-gated compaction swarm**: a multi-agent pattern for **compounding** repository stewardship (plans, tech-debt registers, documentation, refactors, tests) that runs only when **eligibility gates** allow, uses **quorum** before expensive or wide-blast actions, and defaults to **branch + pull-request** delivery with **no automatic merge** to protected lines.

## Goals

- Give implementers a **shared vocabulary** (gates, profiles, conformance, quorum) independent of a single product.
- Separate **eligibility** (“may run”) from **execution** (“is running”) so operators can explain deferrals without treating them as failures.
- Align with **environment-mediated** swarm taxonomies in **[swarm-orchestration-spec](https://github.com/instagrim-dev/swarm-orchestration-spec)** (v1.1); see **01** for explicit mapping. Where coordination uses signals or claims, implementations MAY bind to **[agent-coordination-substrate](https://github.com/instagrim-dev/agent-coordination-substrate)** primitives. This spec adds **idle gating** and **compaction** semantics.

## Non-goals

- Mandate a specific implementation language, runtime, or hosting provider.
- Require fully autonomous merge to production branches in the default conformance profile.
- Replace interactive human-driven development; compaction is **subordinate** to product intent and review.

## Reading order

1. **01** — Where this pattern sits in a swarm taxonomy.  
2. **02** — When compaction may run (including headless/CI profiles).  
3. **03** — What agents read and write to converge work.  
4. **04** — Who decides before expensive steps.  
5. **05** — Safety matrix and defaults.  
6. **06** — Failure and recovery.  
7. **07** — Conformance claims and versioning.

## Terminology

- **Compaction:** Converging many small inconsistencies into fewer, higher-trust artifacts (documentation, plans, code changes with rationale).
- **Idle-gated:** Eligibility requires satisfaction of defined **idle and resource** predicates (see **02**).
- **Swarm (here):** Multiple agents coordinated via **shared artifacts** and explicit **roles**, not only chat fan-out.

## Normative language

**MUST**, **SHOULD**, **MAY** follow [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) sense; profile-specific requirements reference **07**.
