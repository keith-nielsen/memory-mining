<!-- SPDX-License-Identifier: Apache-2.0 -->
## MODIFIED Requirements

### Requirement: Pipeline Stages

Every piece of content SHALL have a defined position on the value chain. Stage names
predict the next stage and the material's current value-state (CONST-01). Prospecting is
the upstream, human act of discovering Claims from the world and is not a Site state — a
Site is born already committed to work, at `dig`.

| Stage | Location | Object | Key field | Meaning |
|---|---|---|---|---|
| Capture | `20-Claims/` | loose note | — | Raw, unsorted input (the inbox) |
| Dig | `30-Sites/<slug>/` | effort | `status: dig` | Active extraction in progress |
| Ore | `30-Sites/<slug>/` | effort | `status: ore` + `grade` | Raw material extracted; grade **estimated** |
| Sort | (process, not folder) | decision | — | 3-way triage |
| Refine | `40-Treasury/` | knowledge note | `stage: refined` | Smelted to bullion; grade confirmed |
| Polish | `40-Treasury/` | knowledge note | `stage: polished` | Perpetual upkeep of bullion |
| Slagged | `70-Tailings/<slug>/` | effort | `status: slagged` + `slag_reason` | Real value, not economic now; retained, re-minable |
| Spent husk | `71-Spoil/<slug>/` | husk | `status: spent` | Residue of a successful refine; terminal |
| Waste | `71-Spoil/<slug>/` | stub | `status: waste` | Proven false or empty; terminal |

Lifecycle verbs: **dig** (Claim→Site), **slag** (Site→Tailings), **dump** (Site→Spoil),
**redig** (Tailings→Site), **refine** (ore→bullion, gated), **bank** (the human gate that
authorizes bullion into the Treasury). **reprospect** is a read-only survey of the
Tailings. The effort `status` set is therefore `dig`, `ore`, `slagged` (Spoil uses
`spent`/`waste`).

#### Scenario: A Site is born at dig
- **WHEN** an operator digs a Claim into a Site
- **THEN** the effort's `_effort.md` is created with `status: dig` (never `prospect`)
- **WHEN** material is extracted and assay'd
- **THEN** the operator sets `status: ore` and an estimated `grade`

#### Scenario: prospect is not a Site status
- **WHEN** the linter or kanban reads effort statuses
- **THEN** the valid set is `dig | ore | slagged` — `prospect` is absent (it is the upstream human inflow, not a state)
