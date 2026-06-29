## 1. Constitution / invariant (direct edits)

- [x] 1.1 `project.md` — append INV-14 (Safety band); header range → INV-1 – INV-14
- [x] 1.2 `constitution.md` — §2 Tier-0 Inviolable list gains INV-14
- [x] 1.3 `access-control` spec frontmatter — `protects:` += INV-14

## 2. Spec deltas

- [x] 2.1 `access-control` — ADDED Requirement "Private by Default — No Unbid Publication (INV-14)"
- [x] 2.2 `maintenance` — MODIFIED Script Inventory (+ `push-guard-script`)

## 3. Enforcement rails (vault-template)

- [x] 3.1 `push-guard-script.md` — deny-by-default `pre-push`, `PUSH_ALLOWLIST`-gated (deterministic, INV-6)
- [x] 3.2 `config.env` + `config.env.example` — `VAULT_PUBLISH_GUARD`, `PUSH_ALLOWLIST` (+ `close-daily`→`daily-close` comment fix)
- [x] 3.3 Portable `.claude/hooks/outbound-publish-guard.py` + `.claude/settings.json` PreToolUse — repo **and** vault-template (keyed on `$VAULT_ROOT`/`$CLAUDE_PROJECT_DIR`)

## 4. Docs

- [x] 4.1 `AGENTS.md` — private-by-default guidance (never propose outbound publication)
- [x] 4.2 `README.md` — counts (18 ADRs, 14 invariants) + "Private by default" section
- [x] 4.3 `docs/USING-THIS-TEMPLATE.md` — Private-by-default + opt-in steps

## 5. Regression

- [x] 5.1 `openspec validate private-by-default-publish-guard --strict` + `--all --strict`
- [x] 5.2 `constitution-lint` + `vocabulary-lint`
- [x] 5.3 guard-script unit checks (deny vault-outward; ASK public publish; allow benign/allowlisted)

## 6. Gate 4 + release (human-gated)

- [x] 6.1 Gate-4 sign-off recorded in `proposal.md` (Keith — INV-14 ratified, full ceremony, 2026-06-29)
- [x] 6.2 `openspec/adr/0018-private-by-default-publish-guard.md` (Accepted)
- [ ] 6.3 Archive (sync specs) + CHANGELOG `[0.1.13]` + tag `v0.1.13`
- [ ] 6.4 Mirror to live vault (deploy `pre-push`, `PUSH_ALLOWLIST=` empty, retire hand-rolled local hook)
