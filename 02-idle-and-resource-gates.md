# 02 — Idle and resource gates

## Composite eligibility (default interactive profile)

For the **interactive default profile** (see **07**), a compaction run **MUST NOT** start unless **all** of the following hold:

1. **Human-idle** — The designated human subject (per deployment: user session, workstation, or org policy) is not actively driving the primary dev surface, per implementation-defined signals and thresholds.
2. **Machine-quiet** — CPU, GPU, memory, disk, or network pressure (as configured) are below thresholds that would materially harm interactive work.
3. **Policy-ok** — Calendar window, organizational policy, token/budget caps, CI stability, or other **policy** predicates pass.

Implementations **SHOULD** use **asymmetric thresholds** (hysteresis): different dwell times or levels for **entering** vs **leaving** idle-qualified state, to avoid flapping from saves, language servers, or brief focus changes.

Implementations **SHOULD** expose a **small state machine** (e.g. busy → settling → idle-qualified) and record **which phase** blocked eligibility when work does not start.

## Headless / CI profile

When no human-idle signal exists (e.g. server, batch host), implementations **MUST** use a **headless or CI profile** documented in **07** that:

- Substitutes or omits **human-idle** with explicit predicates (e.g. maintenance window, job lease, operator lock).
- Retains **machine-quiet** and **policy-ok** (or documents their equivalents).
- Does not claim interactive-default semantics.

## Observability

When eligibility fails, implementations **SHOULD** emit a **stable reason code** and minimal **decision trace** (evaluation order, threshold values or versions) so operators can answer “why didn’t it run?” without reproducing the full environment.

## Fairness and caps

Implementations **SHOULD** enforce **concurrency limits**, **cooldowns** after recent user activity, and **cleanup** of stale child work so compaction does not steal interactive latency or leak resources.

## Informative: BMO implementation notes

For a concrete signal inventory (TUI last-input, run quiet, load sampling, headless substitutes, skip reason codes, and platform matrix), see [ADR-003](https://github.com/instagrim-dev/bmo/blob/main/docs/decisions/003-idle-gates-compaction-spike.md). It is **non-normative** for this spec revision.
