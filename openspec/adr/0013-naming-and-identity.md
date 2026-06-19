<!-- SPDX-License-Identifier: Apache-2.0 -->
# ADR-0013 — Naming & identity: `index` over `moc`, folder-note over `_effort`

**Status:** Accepted
**Date:** 2026-06-19
**Relates:** CONST-05 · `vault-structure` spec · change `naming-and-identity`

## Context

Two conventions failed human-centric design. `<pillar>-moc.md` used "MOC" (Map of Content) —
imported PKM acronym jargon that teaches a newcomer nothing, violating CONST-01's
self-teaching spirit. `30-Sites/<slug>/_effort.md` named *every* Site's main note `_effort`,
so the note was **anonymous** in any flat view (graph, search, Quick Switcher, migration);
its identity lived only in the folder name plus an Obsidian display alias — fragile under a
tooling change or manual browsing ("can't have 60,000 `_effort.md` files swimming around").

## Decision

- **`moc → index`** — the note type, the Catalog notes (`<pillar>-index.md`, `home-index`),
  the mold, the `index_links` proposal field, and CONST-05's label. The *principle* (domain via
  metadata + Catalog, never folders) is untouched.
- **`_effort → <slug>/<slug>.md`** (folder-note) — a Site/Tailings/Spoil effort note's stem
  equals its folder name, so it self-identifies everywhere and is cleanly separable from
  working materials in the same folder. Scripts locate it as "the file whose stem == its
  folder," replacing the fixed `_effort.md` name.

## Options considered

1. **`index` vs `map`/`ledger`/bare `<pillar>.md`** — chose `index` for maximal universality
   (everyone knows it); a self-describing suffix beats a bare name for graph/search legibility.
2. **Folder-note vs flatten `30-Sites/<slug>.md` vs an Obsidian display plugin** — chose the
   folder-note: it keeps the materials folder *and* makes the **source of truth** self-identify,
   independent of any viewer. The display-plugin "fix" was rejected as tool-bound; flattening
   single-note Sites is deferred as a separate question.

## Consequences

- Forks/vaults `git mv` Catalog + effort notes and repoint wikilinks on upgrade (mechanical);
  the `_`-sort-to-top trick on effort notes is replaced by the folder-note convention.
- The artifact layer becomes resilient to migration and manual browsing by construction.
- **Sacrifice:** the one-time migration cost, and the small redundancy of `<slug>/<slug>.md`.
  The durable design principles this surfaced (self-teaching names; identity over anonymity;
  fix-the-source-not-the-display) are mined as ore in the vault for later Treasury banking.
