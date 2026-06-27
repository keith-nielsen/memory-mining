## Context

The four note molds carry generic stems (`daily.md`, `effort.md`, `index.md`, `knowledge.md`).
They are anonymous in any flat/search/migrated view and `index.md` collides with the Catalog
`<pillar>-index.md` notes. This change renames them to the `silo-section-descriptor` form
(`<note-type>-mold-blank.md`). It is a constitution-override **conforming amendment** touching one
`protects:`-tagged spec — `vault-structure` (the Folder Structure mold listing). (A prior ad-hoc
rename in the live vault was reverted; this is the governed redo.)

## Goals / Non-Goals

**Goals:**
- Rename the four molds to `<note-type>-mold-blank.md`.
- Update the single path-consumer (the `daily-note` script), the Obsidian Daily Notes template path, and the doc references — in lockstep.
- Update the `vault-structure` Folder Structure listing.

**Non-Goals:**
- The general ≥3-token rule and any enforcement → change #2 (`naming-convention-3token`).
- Scripts, schemas, Catalog indexes, content names → later changes.
- Renaming the deployed `~/bin/vault-*.py` binaries → deferred (`.py` deferred per Keith).

## Decisions

- **Form = `<note-type>-mold-blank`** (silo=note-type, section=`mold`, descriptor=`blank`). `blank` is constant across molds; its job is cross-vault disambiguation, not intra-folder.
- **`git mv`** for the renames (preserve history).
- **Only `daily-note` reads a mold by path** (verified). Update its code-block path, then `vault-render.py render` to deploy and `reconcile` to confirm zero drift.
- **No `naming-rules` / `maintenance` spec delta** (verified): molds aren't in `naming-rules`' governed slug set and the new stems are valid; `maintenance` names no mold file (the path is implementation, not requirement).

## Risks / Trade-offs

- `daily-note` breaks if the path isn't repointed → repoint + re-render + `reconcile` zero-drift before commit.
- Forks/live vaults must migrate → mechanical `git mv` + path repoint + re-render; recorded as the ADR sacrifice.
- Obsidian "Insert template" pick-list shows the new stems → cosmetic; docs updated.

## Migration Plan

1. `vault-structure` spec delta (done under `specs/`).
2. `git mv` the four molds in `vault-template/97-Molds/`.
3. Repoint `vault-template/99-Operations/scripts/daily-note.md` mold path; re-render; `reconcile` zero-drift.
4. Repoint the Obsidian Daily Notes template path; update `00-Docs/README.md` + `docs/obsidian.md` mold refs.
5. Lints green → `feat(constitution-override): mold-naming … [Gate 3]`.
6. Gate 4 sign-off + ADR-00NN → `archive` + CHANGELOG + tag.
7. Mirror to live vault (`git mv` + daily-note path + re-render + reconcile).

## Open Questions

- None blocking. The general convention + enforcement decision (D1) lives in change #2, not here.
