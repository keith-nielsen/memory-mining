## 1. Spec

- [x] 1.1 `naming-rules` delta — ADDED **Token-Minimum Naming** requirement (≥3 floor, more where warranted; silo-section-descriptor for system artifacts; content ≥3; dailies exempt; grandfather + enforcement-deferred notes)

## 2. Docs

- [x] 2.1 Naming-guidance note added to `AGENTS.md` (the canonical agent context; `CLAUDE.md` already points there) → ≥3 floor, more where specificity warrants, silo-section-descriptor, SSOT pointer

## 3. Regression

- [x] 3.1 `openspec validate --all` green (8/8 items)
- [x] 3.2 `constitution-lint` (6 specs retain `protects:`) green; `vocabulary-lint` unaffected (no `config.env`/GRADES change)
- [x] 3.3 `validate-scripts.sh` green (sandboxed; VALIDATION OK)
- [ ] 3.4 Full CI green on push

## 4. Gate 4 + release (human-gated)

- [x] 4.1 Gate-4 sign-off recorded in `proposal.md` (Keith — "Authorized", 2026-06-27)
- [x] 4.2 `openspec/adr/0015-naming-3token-minimum.md` (Accepted; context/options/choice/consequence/sacrifice)
- [ ] 4.3 `/opsx:archive` + CHANGELOG `[0.1.8]` + tag `v0.1.8`
- [ ] 4.4 Mirror naming-guidance note to the live-vault `CLAUDE.md` (if added); no spec mirror (live vault carries no openspec specs)
