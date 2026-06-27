<!-- SPDX-License-Identifier: Apache-2.0 -->

# Constitution Override: mold-naming

**Change type:** `constitution-override`
**Principle(s) affected:** `vault-structure` spec (Folder Structure mold listing) — `protects: [CONST-02, CONST-04, CONST-05, INV-1, INV-12]`. **Conforming amendment** — only the literal mold filenames in the listing change; no principle text is weakened (layers, folder numbering/order, flat-Treasury, format all stand).
**Tier:** 1
**Proposer:** Keith Nielsen (drafted by Claude Code; authorized by Keith)
**Date:** 2026-06-26

---

## Why

The note molds carry generic single-word stems (`daily.md`, `effort.md`, `index.md`, `knowledge.md`) that are anonymous in any flat / search / migrated view, and `index.md` collides with the seven Catalog `<pillar>-index.md` notes. A self-identifying three-token `silo-section-descriptor` form (`<type>-mold-blank`) makes each mold legible on its own and unmistakable from content. Scoped to **molds only**; the general ≥3-token naming rule (and any enforcement) is change #2 (`naming-convention-3token`).

## What Changes

- Rename the four molds: `daily.md → daily-mold-blank.md`, `effort.md → effort-mold-blank.md`, `index.md → index-mold-blank.md`, `knowledge.md → knowledge-mold-blank.md` (silo = note-type, section = `mold`, descriptor = `blank`).
- **Spec:** update the `97-Molds/` listing in `vault-structure`'s **Folder Structure** requirement.
- **Implementation (vault-template, no requirement change):** repoint the `daily-note` script's mold path (`97-Molds/daily.md → daily-mold-blank.md`); the Obsidian Daily Notes template path; doc references in `00-Docs/README.md` + `docs/obsidian.md`.
- **No principle weakened.** CONST-01's self-teaching spirit and INV-11 are *reinforced*.

## Capabilities

### New Capabilities
- _(none)_

### Modified Capabilities
- `vault-structure`: the four mold filenames in the **Folder Structure** listing change to `<type>-mold-blank.md`. No other requirement changes.

## Impact

- **Spec delta:** `vault-structure` only.
- **Implementation (vault-template, not `protects:`-tagged, no requirement change):** `97-Molds/*.md` (4 renames) · `99-Operations/scripts/daily-note.md` (mold path) · doc mold refs in `00-Docs/README.md`, `docs/method.md`, `docs/obsidian.md`, `docs/USING-THIS-TEMPLATE.md`. (vault-template ships **no** `.obsidian/` config — the Obsidian Daily Notes template path is a **live-vault** concern, handled at mirror.)
- Forks/live vaults (mirror): `git mv` the four molds + repoint the daily-note path + the Obsidian template path + re-render.
- New ADR (Gate 4).

---

## Gate 1 — CHECK (Impact Analysis)

**Principle(s) restated (own words):** `vault-structure` (`protects: CONST-02/04/05, INV-1/12`) fixes where things live and their on-disk shape — the three layers, CONST-04 zero-padded gapped touch-frequency folder numbering, flat metadata-driven Treasury (CONST-05/INV-12), and Markdown+frontmatter format (INV-1). This change edits **only the literal mold filenames** in the Folder Structure listing. It does **not** touch layer membership, folder numbers/order, Treasury flatness, or the format invariant — so no principle is weakened. The new stems are valid kebab slugs under the existing INV-11 rule, so `naming-rules` is untouched.

**Blast radius (checked off in Gate 3):**

- [ ] `openspec/specs/vault-structure/spec.md` — MODIFIED **Folder Structure**: `97-Molds/` listing → `<type>-mold-blank.md`
- [ ] `vault-template/97-Molds/{daily,effort,index,knowledge}.md` → `<type>-mold-blank.md` (`git mv`)
- [ ] `vault-template/99-Operations/scripts/daily-note.md` — mold path in the code block
- [ ] `vault-template/00-Docs/README.md` — mold ref
- [ ] `docs/method.md` — mold refs (×2)
- [ ] `docs/obsidian.md` — mold refs (×2)
- [ ] `docs/USING-THIS-TEMPLATE.md` — mold ref
- [ ] ADR-00NN (new)
- _(vault-template ships no `.obsidian/`; the Obsidian Daily Notes template path is repointed in the **live-vault mirror**, not here.)_

---

## Gate 2 — PLAN (Migration + Regression)

**Migration:** template renames via `git mv`; the `daily-note` script + Obsidian template path repointed in lockstep; docs updated. Forks/live vaults run the same `git mv` + path repoint + re-render.

**Regression that MUST pass before Gate 3:** `openspec validate --all --strict`; `constitution-lint`; `vocabulary-lint`; `validate-scripts.sh`; link-check; CI green.

---

## Gate 3 — EXECUTE + REGRESSION TEST

**Implementation complete:** ☑
**All regression tests green (local suite):** ☑ — `openspec validate --all` 8/8 · constitution-lint (6 specs retain `protects:`) · vocabulary-lint (config vars + GRADES) · `validate-scripts.sh` sandboxed VALIDATION OK (incl. daily-note-from-mold smoke)
**CI green on this PR:** ☐ (runs on push)

---

## Gate 4 — RE-CHECK + HUMAN SIGN-OFF

**Second review confirms blast radius was fully addressed:** ☑
**Consequences explicitly accepted:**

> Sacrifice: forks/vaults must `git mv` four molds + repoint the `daily-note` path on upgrade (mechanical). No principle, invariant, or workflow is weakened.

**ADR created:** `openspec/adr/0014-mold-naming.md` ☑
**ADR captures:** context / options / choice / consequence / **sacrifice** ☑

**SIGN-OFF** (human only — agents may not sign):
Name: **Keith Nielsen** — "Authorized." (transcribed by Claude Code at Keith's explicit instruction; the decision is the human's)
Date: 2026-06-27
