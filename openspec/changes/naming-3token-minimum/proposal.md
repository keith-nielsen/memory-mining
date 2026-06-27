<!-- SPDX-License-Identifier: Apache-2.0 -->

# Constitution Override: naming-3token-minimum

**Change type:** `constitution-override`
**Principle(s) affected:** `naming-rules` spec — `protects: [INV-11]`. **Conforming amendment** — purely additive (a new requirement); it *strengthens* INV-11 and weakens nothing.
**Tier:** 0 (INV-11) — additive/conforming
**Proposer:** Keith Nielsen (drafted by Claude Code; authorized by Keith)
**Date:** 2026-06-27

---

## Why

Filenames need enough disambiguating context as the namespace grows. A **≥3-token floor — three is the floor, not the ceiling — with more wherever the extra tokens add human-meaningful specificity** pre-empts the overloading/collisions that arise as broad topics narrow and digs reveal sub-sectors. Codify the rule now as **convention**; mechanical enforcement is sequenced into a later change once the families conform.

## What Changes

- **ADD** a *Token-Minimum Naming* requirement to `naming-rules`: ≥3-token stems (more where specificity warrants); system-artifact families use **`silo-section-descriptor`** (silo first); content stems are ≥3-token topic slugs; dailies (`YYYY-MM-DD`) exempt. Existing sub-3 names are **grandfathered**; each family conforms via its own change (molds shipped in v0.1.7).
- **Convention-level explicitly:** mechanical enforcement (extending `vault_naming.py` + the commit-gate hook + `naming-rules.json` to *reject* sub-3 stems) is **deferred** to a later change, after conformance.
- Document the rule for agents in `CLAUDE.md` / `AGENTS.md` (repo).
- **No existing requirement changed; no name renamed in this change.**

## Capabilities

### New Capabilities
- _(none)_

### Modified Capabilities
- `naming-rules`: ADD the *Token-Minimum Naming* requirement (additive).

## Impact

- **Spec delta:** `naming-rules` (one ADDED requirement).
- **Docs:** a naming-guidance note in `CLAUDE.md` / `AGENTS.md` (repo).
- **No** file renames, **no** validator/hook/`naming-rules.json` change (deferred).
- New ADR (Gate 4). Mirror to live vault is minimal (a `CLAUDE.md` naming note, if any).

---

## Gate 1 — CHECK (Impact Analysis)

**Principle(s) restated (own words):** INV-11 requires every name to conform to the ruleset, enforced deterministically at the boundary (commit-gate hook + refine executor); breaking it yields unsafe or illegible names. This change **adds** a token-minimum convention on top of the existing kebab-safety rules — purely additive. Nothing previously valid becomes invalid: existing sub-3 names are grandfathered, the floor applies to new/renamed names by convention, and the boundary validator is **unchanged** (mechanical enforcement is a separate, later change). INV-11 is reinforced, not weakened.

**Blast radius (checked off in Gate 3):**

- [ ] `openspec/specs/naming-rules/spec.md` — ADDED **Token-Minimum Naming** requirement
- [ ] `CLAUDE.md` and/or `AGENTS.md` — naming-guidance note (repo)
- [ ] ADR-00NN (new)

---

## Gate 2 — PLAN (Migration + Regression)

**Migration:** none — additive spec + a doc note; no renames, no validator change. Family conformance lands via separate changes (`script-naming`, `schema-naming`, `index-naming`, content). Mechanical enforcement is a future change, gated on full conformance.

**Regression that MUST pass before Gate 3:** `openspec validate --all --strict`; `constitution-lint`; `vocabulary-lint`; `validate-scripts.sh`; CI green.

---

## Gate 3 — EXECUTE + REGRESSION TEST

**Implementation complete:** ☑ — `naming-rules` ADDED requirement + `AGENTS.md` naming note
**All regression tests green (local suite):** ☑ — `openspec validate --all` 8/8 · constitution-lint (6 specs retain `protects:`) · vocabulary-lint unaffected · `validate-scripts.sh` sandboxed VALIDATION OK
**CI green on this PR:** ☐ (runs on push)

---

## Gate 4 — RE-CHECK + HUMAN SIGN-OFF

**Second review confirms blast radius was fully addressed:** ☐
**Consequences explicitly accepted:**

> Sacrifice: none structural — the rule is convention-level until a later change wires mechanical enforcement; no name is renamed and no principle is weakened. The cost deferred to that later change is bringing the remaining families (scripts/schemas/indexes/content) into conformance before rejection can be turned on.

**ADR created:** `openspec/adr/00NN-naming-3token-minimum.md` ☐
**ADR captures:** context / options / choice / consequence / **sacrifice** ☐

**SIGN-OFF** (human only — agents may not sign):
Name: ___________________________
Date: ___________________________
