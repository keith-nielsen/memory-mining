## 1. Spec

- [x] 1.1 `vault-structure` delta — MODIFIED **Folder Structure**: `97-Molds/` listing → `<note-type>-mold-blank.md` (authored under `specs/vault-structure/spec.md`)

## 2. Template implementation (vault-template)

- [x] 2.1 `git mv` the four molds → `daily-mold-blank.md`, `effort-mold-blank.md`, `index-mold-blank.md`, `knowledge-mold-blank.md`
- [x] 2.2 Repoint the mold path in `99-Operations/scripts/daily-note.md` code block (`97-Molds/daily.md` → `daily-mold-blank.md`). Re-render/reconcile is a **live-vault** step (mirror) — vault-template has no `~/bin` deployment.
- [x] 2.3 Update mold filename refs in docs: `00-Docs/README.md`, `docs/method.md` (×2), `docs/obsidian.md` (×2), `docs/USING-THIS-TEMPLATE.md`
- [x] 2.4 Grep: no residual mold-filename refs (`97-Molds/<type>.md`) outside archive/history

## 3. Regression

- [x] 3.1 `openspec validate --all` green (8/8 items)
- [x] 3.2 `constitution-lint` (6 specs retain `protects:`) + `vocabulary-lint` (config vars + GRADES) green
- [x] 3.3 `validate-scripts.sh` green (sandboxed; VALIDATION OK, incl. daily-note-from-mold smoke)
- [ ] 3.4 Full CI green on push (incl. link-check, runbook-lint, spec-lint)

## 4. Gate 4 + release (human-gated)

- [x] 4.1 Gate-4 sign-off recorded in `proposal.md` (Keith — "Authorized", 2026-06-27)
- [x] 4.2 `openspec/adr/0014-mold-naming.md` (Accepted; context/options/choice/consequence/sacrifice)
- [ ] 4.3 `/opsx:archive` + CHANGELOG `[0.1.x]` + tag `v0.1.x`
- [ ] 4.4 Mirror to live vault (`git mv` molds + daily-note path + Obsidian Daily Notes template path + re-render + reconcile zero-drift)
