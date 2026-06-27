## Context

The vault is adopting a ≥3-token naming floor (more where specificity warrants), with
`silo-section-descriptor` for system-artifact families — to keep names deconflicted as the namespace
grows. The molds already conform (v0.1.7 / ADR-0014). This change codifies the **general rule** in the
`naming-rules` spec. D1 is settled: **convention now, mechanical enforcement later.**

## Goals / Non-Goals

**Goals:**
- Add one *Token-Minimum Naming* requirement to `naming-rules` (the SSOT for the rule).
- Note the rule for agents in `CLAUDE.md` / `AGENTS.md`.

**Non-Goals:**
- Mechanical enforcement (validator / commit-gate hook / `naming-rules.json`) — a later change, after conformance.
- Renaming any family (scripts/schemas/indexes/content) — each is its own change.

## Decisions

- **Convention now, enforcement later.** Turning on rejection while sub-3 names exist would block all
  commits until every family is renamed (indexes are also gated on the descriptor decision). So the
  rule lands as documented convention; a later change extends the validator once the namespace conforms.
- **Floor, not ceiling.** ≥3 minimum; *more* where tokens add human-meaningful specificity. Captured
  explicitly so the rule isn't misread as "exactly 3."
- **Scope = all `.md`.** System artifacts → `silo-section-descriptor`; content → ≥3-token topic slugs
  (longer correct); dailies exempt. Existing sub-3 names grandfathered.

## Risks / Trade-offs

- A SHALL that isn't mechanically enforced relies on discipline → mitigated by (a) explicit
  convention-stage + grandfathering language in the spec, (b) the agent guidance note, and (c) the
  reboot load-list ([[llm-context-reboot]]) which makes the rule a loaded constraint, not recollection.
- Drift between "rule stated" and "families conformant" → bounded: each family has its own queued
  change; the eventual enforcement change is the backstop.

## Migration Plan

None. Additive spec + a doc note. No renames, no validator change.

## Open Questions

- The eventual **mechanical-enforcement** change: its trigger (after which families conform) and the
  exact validator/hook extension — out of scope here, sequenced later.
