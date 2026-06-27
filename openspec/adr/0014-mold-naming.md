<!-- SPDX-License-Identifier: Apache-2.0 -->
# ADR-0014 — Mold naming: `<note-type>-mold-blank` (silo-section-descriptor)

**Status:** Accepted
**Date:** 2026-06-27
**Relates:** `vault-structure` spec · change `mold-naming` · extends ADR-0013 (naming-and-identity)

## Context

The four note molds carried generic single-word stems — `daily.md`, `effort.md`, `index.md`,
`knowledge.md`. Two problems: they are **anonymous** in any flat / search / migrated view (a bare
`index.md` tells a reader nothing), and `index.md` **collides** with the seven Catalog
`<pillar>-index.md` notes. This continues the human-centric naming work begun in ADR-0013
(`moc → index`, `_effort → folder-note`).

## Decision

Rename the molds to the **`silo-section-descriptor`** form `<note-type>-mold-blank.md`:
`daily-mold-blank.md`, `effort-mold-blank.md`, `index-mold-blank.md`, `knowledge-mold-blank.md`
(silo = note-type, section = `mold`, descriptor = `blank`). The `vault-structure` Folder Structure
listing is updated; the one path-consumer (`daily-note` script), the Obsidian Daily Notes template
path, and the doc references are repointed in lockstep.

Scoped to **molds only**. The general "≥3 tokens (floor, not ceiling; more where specificity
warrants)" naming rule and any mechanical enforcement are a separate change (`naming-convention-3token`).

## Options considered

1. **`<type>-mold-blank` vs `vault-<type>-mold` vs `mold-<type>`** — chose silo-first
   `<type>-mold-blank`: the discriminating token leads (fast recognition / prefix-completion), the
   shared `-mold-blank` tail regroups the family under a glob if flattened, and the folder is not
   stuttered against (`97-Molds/mold-…`). `blank` is a constant descriptor whose job is cross-vault
   disambiguation against homonyms (e.g. a future health note about household *mold*), not
   intra-folder distinction.
2. **Convention-level for molds vs mechanical enforcement now** — chose convention here; the molds
   are valid kebab slugs under the existing INV-11 rule, so `naming-rules` is untouched. Mechanical
   ≥3 enforcement is sequenced into a later change once all families conform (it would otherwise
   reject still-unrenamed sub-3 names).

## Consequences

- Molds self-identify in any flat / search / migrated view and never collide with content.
- Forks/live vaults `git mv` four molds + repoint the `daily-note` path (and, in a live vault, the
  Obsidian Daily Notes template path) + re-render — mechanical.
- **Sacrifice:** the one-time migration cost and the small redundancy of the `-mold-blank` tail. No
  principle, invariant, or workflow is weakened; CONST-01 (self-teaching) and INV-11 are reinforced.
