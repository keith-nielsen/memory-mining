<!-- SPDX-License-Identifier: Apache-2.0 -->

# Constitution Override: lifecycle-vocabulary

**Change type:** `constitution-override`  
**Principle(s) affected:** CONST-01 (The Value Mining Metaphor)  
**Tier:** 1  
**Proposer:** Keith Nielsen  
**Date:** 2026-06-15

---

## Why

Through extended metaphor design we crystallized the lifecycle verbs and found one
implementation imprecision: **`prospect` was modeled as a Site status**, but prospecting
is the *upstream, human, unbounded* act of discovering value in the world — it *produces*
Claims, it doesn't operate on Sites. A Site is born already committed to work. Retiring
`prospect` as a status removes a phantom state and lets the verbs line up cleanly.

## What Changes

- **`prospect` retired as a Site status.** Sites are born at `dig`; statuses become
  `dig → ore` (Tailings `slagged`; Spoil `spent`|`waste`). `prospect` remains a *named
  upstream act* (human inflow), not a state — un-modeled for now.
- **Verb set locked:** `dig` (Claim→Site), `slag` (Site→Tailings), `dump` (Site→Spoil,
  renamed from `dispose`), `redig` (Tailings→Site), `refine` (ore→bullion, gated),
  **`bank`** (human gate, ore→Treasury; state `authorized`).
- **`reprospect` reclassified** as the one automatable *survey* (bounded, AI-ownable),
  sibling of `orphans` — not a transition.
- CONST-01's *chain* is unchanged; this sharpens its *implementation*.

---

## Gate 1 — CHECK (Impact Analysis)

**Principle restated:** CONST-01 says each pipeline stage name lets you infer the next
stage and the material's value-state. Treating `prospect` as a Site value-state was wrong
— nothing of value sits "in prospect"; prospecting is how Claims come to exist. "What
breaks" if the vocabulary is arbitrary: stage names stop predicting state, and every
script/spec/diagram that encodes the status set drifts. This change *strengthens* CONST-01.

**Blast radius:**

- [ ] `openspec/specs/value-pipeline/spec.md` — Pipeline Stages table + scenario (`prospect` row/initial status)
- [ ] `openspec/specs/maintenance/spec.md` — kanban column scenario
- [ ] `vault-template/99-Operations/config.env` — `EFFORT_STATUSES`
- [ ] `vault-template/97-Molds/effort.md` — default `status`
- [ ] `vault-template/99-Operations/scripts/kanban-render.md` — status columns
- [ ] `vault-template/99-Operations/scripts/dispose.md → dump.md` — rename + `deploy_target`
- [ ] `vault-template/99-Operations/schemas/frontmatter.md` — status enum
- [ ] `docs/glossary.md` — new Lifecycle Vocabulary table + term updates
- [ ] `docs/method.md`, `docs/diagrams.md` — Stage 2 rewrite; Effort-Lifecycle + Value-Chain diagrams
- [ ] `README.md`, `AGENTS.md`, `vault-template/CLAUDE.md`, `00-Docs/README.md` + examples — pipeline prose
- [ ] `openspec/adr/0010-lifecycle-vocabulary.md` (new) + `CHANGELOG.md [0.1.4]`

---

## Gate 2 — PLAN (Migration + Regression)

1. Retire `prospect` status across config/mold/kanban/schema/specs.
2. Rename `dispose → dump` (note + deploy_target + all refs).
3. Reframe `reprospect` as a survey; document `dig`/`redig`/`bank` + `authorized`.
4. Write `docs/glossary.md` Lifecycle Vocabulary; update narrative + diagrams.
5. ADR-0010 + CHANGELOG `[0.1.4]`.
6. Regression: `openspec validate --all --strict`, link-check, `validate-scripts.sh`.
7. Commit → CI → tag `v0.1.4` → archive → mirror to the live vault.

**Regression tests that MUST pass:**
- [ ] `openspec validate --all --strict`
- [ ] `constitution-lint`, `vocabulary-lint`, `spec-lint`, `link-check`
- [ ] `validate-scripts.sh` (Py 3.12/3.13) — render, zero-drift, daily-note, lint, refine-detect, kanban (no `prospect` column), INV-11 boundary, `vault-dump.sh` present

---

## Gate 3 — EXECUTE + REGRESSION TEST

**Implementation complete:** ☑  
**All regression tests green:** ☑  
**CI green:** ☑ (confirmed on the v0.1.4 push before tag/archive)  

---

## Gate 4 — RE-CHECK + HUMAN AUTHORIZE

**Second review confirms blast radius addressed:** ☑  
**Consequences accepted:**

> `prospect` is no longer a Site status; the effort mold, `EFFORT_STATUSES`, and the kanban
> drop it. `dispose` is renamed `dump`. Existing forks/vaults must migrate the status set
> and the script name. CONST-01's principle text is unchanged — implementation sharpened,
> not sacrificed.

**ADR created:** `openspec/adr/0010-lifecycle-vocabulary.md` ☑  

**AUTHORIZE** (human only — agents may not authorize):  
Name: **Keith Nielsen** — *"I have authorized the whole thing"* (prefers "authorized" to "signed off"), 2026-06-15.  
Date: **2026-06-15**  
