<!-- SPDX-License-Identifier: Apache-2.0 -->
# Tasks — swap-logbook-claims-order

> Do not start until Gate 4 SIGN-OFF is recorded in `proposal.md` (human-only).

## Repo (authoritative)
- [ ] `git mv` `vault-template/20-Logbook → 10-Logbook` and `10-Claims → 20-Claims`
- [ ] Two-pass substitution on mechanical-set files (excl. CHANGELOG, source/)
- [ ] Hand-edit structural files: vault-structure tree, diagrams Folder Stack, README layout, 00-Docs layout
- [ ] Apply `vault-structure` spec delta to the live spec
- [ ] Add `openspec/adr/0009-layer2-ordering-correction.md`
- [ ] Add `CHANGELOG.md [0.1.3]`
- [ ] Regression: `openspec validate --all --strict`, link-check, `validate-scripts.sh`
- [ ] Commit, push, confirm CI green, tag `v0.1.3`
- [ ] Archive this change to `openspec/changes/archive/`

## Live vault (`~/Documents/Vault`)
- [ ] Backup tarball
- [ ] `git mv` both directories
- [ ] Re-copy changed meta-scripts + `vault-render.py render`
- [ ] `reconcile` zero drift, `lint` clean, grep for stale paths
- [ ] One structured commit
