# Tasks — spec-as-code-runbooks

## Specs (deltas → live)
- [ ] `vault-structure` delta: `96-Runbooks` band + reserved note `90–95`; `runbook` schema + daily `closed:`
- [ ] `maintenance` delta: Runbook Format + Daily Close Lifecycle requirements; Script Inventory `vault-close-day`
- [ ] Apply both deltas to the live specs

## Band + schema + vocab
- [ ] `vault-template/96-Runbooks/` band
- [ ] `99-Operations/schemas/runbook.md` (meta-schema)
- [ ] `99-Operations/schemas/frontmatter.md` daily `closed:`
- [ ] `99-Operations/config.env` `DISPOSITIONS`

## Charter runbooks
- [ ] `96-Runbooks/seal-provenance.md`
- [ ] `96-Runbooks/close-daily.md`

## Deterministic scripts
- [ ] `99-Operations/scripts/close-daily.md` → `vault-close-day.py`
- [ ] `rollover.md` + `daily-note.md` gate updates (capture-stub always; rollover gated; BLOCKED banner)

## Mold
- [ ] `97-Molds/daily.md` `closed:` + `## Close` / `## Carry-over`

## ADRs
- [ ] `0011-spec-as-code-runbooks.md`
- [ ] `0012-daily-close-lifecycle.md`

## CI teeth + adapters
- [ ] `.github/scripts/runbook-lint`, `close-lint`; `ci.yml` jobs
- [ ] `validate-scripts.sh` adds `vault-close-day`
- [ ] `AGENTS.md` Runbooks pointer + footguns; `CLAUDE.md` pointer

## Gate + release
- [ ] Regression green (openspec --strict, lints, validate-scripts, link-check, CI)
- [ ] **Gate 4 — Keith AUTHORIZE**
- [ ] `CHANGELOG [0.1.5]`; archive change; tag `v0.1.5` + release
- [ ] Mirror to live vault (~/Documents/Vault)
