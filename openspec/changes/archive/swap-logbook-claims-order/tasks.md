<!-- SPDX-License-Identifier: Apache-2.0 -->
# Tasks — swap-logbook-claims-order

> Do not start until Gate 4 SIGN-OFF is recorded in `proposal.md` (human-only).

## Repo (authoritative)
- [x] `git mv` `vault-template/20-Logbook → 10-Logbook` and `10-Claims → 20-Claims`
- [x] Two-pass substitution on mechanical-set files (excl. CHANGELOG, source/)
- [x] Hand-edit structural files: vault-structure tree, diagrams Folder Stack, README layout, 00-Docs layout
- [x] Apply `vault-structure` spec delta to the live spec
- [x] Add `openspec/adr/0009-layer2-ordering-correction.md`
- [x] Add `CHANGELOG.md [0.1.3]`
- [x] Regression: `openspec validate --all --strict`, link-check, `validate-scripts.sh`
- [x] Commit, push, confirm CI green, tag `v0.1.3`
- [x] Archive this change to `openspec/changes/archive/`

## Live vault (`~/Documents/Vault`)
- [x] Backup tarball
- [x] `git mv` both directories
- [x] Re-copy changed meta-scripts + `vault-render.py render`
- [x] `reconcile` zero drift, `lint` clean, grep for stale paths
- [x] One structured commit
