<!-- SPDX-License-Identifier: Apache-2.0 -->

# Constitution Override: naming-and-identity

**Change type:** `constitution-override`
**Principle(s) affected:** CONST-05 (Domain via Metadata + Catalog) — label only; `protects:` specs `vault-structure`, `agent-integration`. **Conforming amendment** (no principle weakened).
**Tier:** 1
**Proposer:** Keith Nielsen (drafted by Claude Code; authorized by Keith)
**Date:** 2026-06-19

---

## Why

Two conventions fail human-centric design. `<pillar>-moc.md` is opaque PKM acronym jargon — it teaches a newcomer nothing (against CONST-01's self-teaching spirit). `30-Sites/<slug>/_effort.md` makes every Site's main note **anonymous** in any flat view (graph / search / migration); identity lived only in the folder name + an Obsidian alias — fragile under tooling change and manual browsing ("can't have 60,000 `_effort.md` files swimming around"). Fix: intuitive names (`index`) and **self-identifying** artifacts (folder-notes).

## What Changes

- **`moc → index`:** the `moc` note type, the `<pillar>-moc.md` Catalog notes, the mold, the `moc_links` proposal field, and CONST-05's label "(MOCs)" → "(indexes)". The *principle* (domain via metadata + Catalog, never folders) is unchanged.
- **`_effort → <slug>/<slug>.md`:** a Site/Tailings/Spoil effort note is now the **folder-note** (stem == folder). Self-identifying; cleanly separated from working materials. The single-source-of-truth principle is unchanged.

---

## Gate 1 — CHECK (Impact Analysis)

**Principle restated:** CONST-05 says domains are expressed via metadata + a Catalog of index notes, never via folders; "what breaks" if overridden is multi-pillar membership + portability. **This change touches only the *label* ("MOC" → "index") and the effort-note *filename*, not the principle** — flat Treasury, metadata-driven pillars, single-source effort note all stand. CONST-01's self-teaching spirit is *strengthened*.

**Blast radius (checked off in Gate 3):**

- [x] `openspec/specs/vault-structure/spec.md` — Frontmatter Schemas (effort/spoil location, `index` row); Folder Structure (mold `moc.md → index.md`)
- [x] `openspec/specs/{agent-integration,maintenance,access-control}/spec.md` — `moc_links → index_links`; MOC→index terminology
- [x] `openspec/constitution.md` — CONST-05 label
- [x] `vault-template/40-Treasury/Catalog/<pillar>-moc.md → -index.md` (7) + `type: moc → index`
- [x] `vault-template/97-Molds/moc.md → index.md`; `vault-template/99-Operations/schemas/frontmatter.md` (`moc`→`index`, effort/spoil location)
- [x] scripts: `rollover`, `refine-detect`, `kanban-render`, `reprospect`, `close-daily` (effort folder-note glob/link), `refine-execute` + `schemas/refine-prompt` (`index_links`), `orphans` (terminology)
- [x] `vault-template/00-Docs/**` examples (incl. `02-site-walkthrough/_effort.md → folder-note`, `03-refine-proposal.json` `index_links`)
- [x] docs: `README`, `docs/{method,obsidian,glossary,USING-THIS-TEMPLATE,diagrams}`, `openspec/project.md`, `vault-template/CLAUDE.md`
- [x] ADR-0013; `.github/scripts/validate-scripts.sh` (`index_links` smoke + folder-note glob)

---

## Gate 2 — PLAN (Migration + Regression)

**Migration:** template renames via `git mv`; forks/live vaults run the same `git mv` + a wikilink repoint (`[[…/<slug>/_effort|…]] → […/<slug>/<slug>…]`, `[[<pillar>-moc…]] → [[<pillar>-index…]]`) + re-render.

**Regression that MUST pass:** `openspec validate --all --strict`; `constitution-lint`; `runbook-lint`; `close-lint`; `validate-scripts.sh` 3.12/3.13 (folder-note glob + `index_links`); link-check; CI green.

---

## Gate 3 — EXECUTE + REGRESSION TEST

**Implementation complete:** ☑
**All regression tests green:** ☑
**CI green on this PR:** ☑

---

## Gate 4 — RE-CHECK + HUMAN SIGN-OFF

**Second review confirms blast radius was fully addressed:** ☑
**Consequences explicitly accepted:**

> Sacrifice: forks/vaults must `git mv` Catalog + effort notes and repoint wikilinks on upgrade (mechanical). The `_` sort-to-top trick on effort notes is lost (folder-note convention replaces it). No principle, invariant, or workflow is weakened.

**ADR created:** `openspec/adr/0013-naming-and-identity.md` ☑
**ADR captures:** context / options / choice / consequence / **sacrifice** ☑

**AUTHORIZE** (human only — agents may not sign):
Name: **Keith Nielsen** — "I've reviewed the plan … the constitution-level change. Authorized."
Date: 2026-06-19
