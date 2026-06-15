<!-- SPDX-License-Identifier: Apache-2.0 -->
# ADR-0010 — Lifecycle Vocabulary: prospect is upstream, not a Site status

**Status:** Accepted  
**Date:** 2026-06-15  
**Amends:** ADR-0002 (Value Mining metaphor) · **Principle:** CONST-01

## Context

Extended metaphor design crystallized the lifecycle verbs and exposed one implementation
imprecision: **`prospect` was modeled as a Site `status`** (`prospect → dig → ore`). But
prospecting is the *upstream, human, unbounded* act of discovering value in the world — it
*produces* Claims; it does not operate on Sites. Nothing of value ever "sits in prospect."

A second clarity emerged — an **agency gradient**: prospecting the open world is unbounded
(human-only); reprospecting a *known* Tailings set against the *known* pipeline state is a
bounded, closed-world problem (automatable). That is the real reason `reprospect` earns a
script and `prospect` does not.

## Decision

- **Retire `prospect` as a Site status.** A Site is born at `dig`; the effort status set is
  `dig | ore | slagged` (Spoil: `spent | waste`).
- **Lock the transition verbs:** `dig` (Claim→Site), `slag` (Site→Tailings),
  `dump` (Site→Spoil, renamed from `dispose`), `redig` (Tailings→Site),
  `refine` (ore→bullion), **`bank`** (the human gate that authorizes bullion into the
  Treasury; state `authorized`).
- **`reprospect`** is the lone automatable **survey** (read-only), sibling of `orphans` —
  not a transition. **`prospect`** is a named upstream act with no script (deferred).
- The canonical chain becomes `Claim → Dig → Ore → Sort → Refine → Bank → Treasury → Polish`.

## Options considered

1. **Retire prospect + lock verbs (chosen).** Removes a phantom state; verbs line up with
   statuses; the agency gradient justifies the script asymmetry.
2. **Keep prospect as a Site status.** Rejected — names stopped predicting state.
3. **Add a `prospect` script for symmetry with `reprospect`.** Rejected — the inbox is
   self-surfacing; the open-world search is not automatable today. "Enough complexity,
   not one drop more."

## Consequences

- CONST-01's *chain* is updated (prospect → upstream; `bank` named at the gate); its
  *principle* — names predict state — is **strengthened**, not sacrificed.
- **Sacrifice / cost:** existing forks/vaults migrate the status set (drop `prospect`,
  mold default `dig`) and rename `vault-dispose.sh → vault-dump.sh`.
- Recorded under constitution-override change `lifecycle-vocabulary`, **authorized** by
  Keith Nielsen 2026-06-15 (who prefers "authorized" to "signed off" — fitting, given
  `authorize` is now the word for banking value into a vault).
