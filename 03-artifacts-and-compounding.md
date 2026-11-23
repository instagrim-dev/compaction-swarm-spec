# 03 — Artifacts and compounding

## Shared artifacts

Compaction swarm agents **SHOULD** coordinate through **durable artifacts** visible to humans and tools, for example:

- Tech-debt or workstream registers (open, in progress, closed).
- Plan or design drafts linked to scope identifiers.
- Documentation deltas with explicit rationale.
- Test or fuzz intents and results (pass/fail, scope).

## Compaction rules

Implementations **SHOULD** define how duplicate or overlapping proposals **merge**:

- Prefer **one canonical record** per logical debt item or initiative.
- **Close or supersede** stale entries rather than appending unbounded duplicates.
- Preserve **audit trail** (who/what proposed, when) where policy requires it.

## Stigmergy

Agents **MAY** use artifact updates as the primary broadcast mechanism; direct agent-to-agent messaging **MAY** supplement but **SHOULD NOT** be the only source of truth for scope and decisions.

## Non-normative examples

Debt ledger files, ADR directories, issue templates, or database tables are all valid stores; the spec does not mandate a format.

## Informative: BMO implementation binding

In BMO, the built-in executor (`internal/compaction/executor`, recipe ref `builtin://compaction`) is what *produces* the shared artifacts above: it selects a bounded candidate, runs one unattended proposer pass, validates, and — after quorum (§04) — commits or promotes a branch/PR rather than mutating protected refs (§05). Artifact stores in practice include debt/workstream registers, plan and doc deltas, and a `COMPACTION.md` rationale trail. This binding is **informative**: the spec does not mandate a runtime (see [00 non-goals](00-overview.md#non-goals)); for current behavior see the [compaction topic](https://github.com/instagrim-dev/bmo/blob/main/docs/topics/agent/compaction.md).
