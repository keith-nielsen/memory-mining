<!-- SPDX-License-Identifier: Apache-2.0 -->
# vault-structure — delta: naming-and-identity

## MODIFIED Requirements

### Requirement: Frontmatter Schemas

Each note type SHALL have an exact required frontmatter schema. The schemas are documented
in `vault-template/99-Operations/schemas/frontmatter.md` and enforced by the linter.

| Type | Location | Key fields |
|---|---|---|
| `knowledge` | `40-Treasury/*.md` | type, title, pillars, grade, stage, crucible, created, updated |
| `index` | `40-Treasury/Catalog/<pillar>-index.md` | type, pillar, created, updated |
| `effort` | `30-Sites/<slug>/<slug>.md`, `70-Tailings/<slug>/<slug>.md` | type, title, status, grade, pillars, started |
| `daily` | `10-Logbook/Daily/YYYY-MM-DD.md` | type, date, closed |
| `meta-script` | `99-Operations/scripts/*.md` | type, deploy_target, runtime, class, created, updated |
| `runbook` | `96-Runbooks/*.md` | type, id, title, trigger, applies-to, class, last-validated |
| `spoil` | `71-Spoil/<slug>/<slug>.md` | type, title, status (spent\|waste), grade, pillars, dumped |

A Site/Tailings/Spoil effort note is the **folder-note** — its stem equals its folder name
(`<slug>/<slug>.md`) — so the note self-identifies in any flat view and is cleanly separable
from working materials in the same folder. The Catalog **index** notes (one per pillar, plus
`home-index`) replace the former `-moc` naming; `type: index` replaces `type: moc`.

#### Scenario: Linter validates knowledge note frontmatter
- **WHEN** the linter runs on a `40-Treasury/*.md` file
- **THEN** it exits 0 for a valid note and exits 1 if `pillars` contains an out-of-set value, `grade` is not one of the four grades, or `stage` is not `refined`/`polished`

#### Scenario: An effort note is the folder-note
- **WHEN** a Site `30-Sites/<slug>/` is created
- **THEN** its effort note is `30-Sites/<slug>/<slug>.md` (stem == folder), and the maintenance scripts identify it as the folder-note rather than a fixed `_effort.md` name

#### Scenario: A Catalog index validates as type index
- **WHEN** the linter parses `40-Treasury/Catalog/<pillar>-index.md`
- **THEN** its frontmatter is `type: index` with a `pillar` field; no `moc` type remains
