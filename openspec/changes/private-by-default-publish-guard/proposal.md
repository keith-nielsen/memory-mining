<!-- SPDX-License-Identifier: Apache-2.0 -->

# Constitution Override: private-by-default-publish-guard

**Change type:** `constitution-override`
**Principle(s) affected:** **ADDS `INV-14` (Tier-0, Safety band)** — private-by-default deployment / no unbid publication. Touches `constitution.md` (§2 Tier-0 table), `project.md` (invariant list), `access-control` spec (`protects:` += INV-14 + new Requirement), `maintenance` spec (Script Inventory gains `push-guard-script`). **Additive — no existing principle weakened; the Safety band is strengthened.** Extends ADR-0008 (frozen INV IDs — this *appends* INV-14, never renumbers 1–13).
**Tier:** 0 — Inviolable (new invariant)
**Proposer:** Keith Nielsen (drafted by Claude Code; authorized by Keith — INV-14 wording ratified 2026-06-29)
**Date:** 2026-06-29

---

## Why

A deployed vault holds private, often irreversible-if-leaked material (Claims, banked Treasury, dailies, confidential IP). Nothing structural currently prevents an automated actor — or a tired human led by one — from pushing that content to a public or external remote. The cost of such a mistake is unbounded and unrecoverable (public data is cached, mirrored, indexed); the value of the convenience is ~zero. This change makes **private-by-default** a Tier-0 guarantee enforced structurally, and — deliberately — makes the guarantee **self-defending**: weakening or removing it later itself requires the full Informed-Upheaval ceremony.

## What Changes

- **New `INV-14` (Tier-0):** *Private by default; no unbid publication.* A deployed vault is private. No automated actor (agent or script) may push, mirror, or otherwise replicate vault content to any remote, except a destination the operator has explicitly allowlisted (`PUSH_ALLOWLIST`). Creating a public repository or publishing to an external distribution hub from within the vault requires explicit, deliberate human confirmation — never an agent's unprompted suggestion. Enforced structurally (deny-by-default push-guard hook + agent-harness outbound guard), never by trust.
- **Enforcement (forkable):** a new rendered `push-guard-script` → `99-Operations/hooks/pre-push` (deny-by-default, allowlist-gated); a portable Claude Code `PreToolUse` guard in `.claude/` (deny vault-outward, loud ASK on any public publish); `config.env` keys `VAULT_PUBLISH_GUARD` / `PUSH_ALLOWLIST`.
- **Docs:** README "Private by default" section + AGENTS.md + USING-THIS-TEMPLATE.

## Capabilities

### New Capabilities
- _(none — INV-14 lands in the existing `access-control` capability)_

### Modified Capabilities
- `access-control`: ADD Requirement "Private by Default — No Unbid Publication (INV-14)"; `protects:` gains INV-14.
- `maintenance`: Script Inventory gains `push-guard-script` (deploy target `99-Operations/hooks/pre-push`).

## Impact

- **Constitution/governance (direct edits):** `openspec/constitution.md` (§2 Tier-0 list += INV-14), `openspec/project.md` (Safety band += INV-14; header "INV-1 – INV-14"), `access-control` spec frontmatter (`protects:` += INV-14).
- **Spec deltas (synced at archive):** `access-control` (ADDED Requirement), `maintenance` (MODIFIED Script Inventory).
- **Implementation (vault-template):** `push-guard-script.md`; `config.env` + `config.env.example` keys (and a stray `close-daily`→`daily-close` comment fix carried over from v0.1.12); `.claude/hooks/outbound-publish-guard.py` + `.claude/settings.json` PreToolUse (repo **and** vault-template, portable via `$VAULT_ROOT`/`$CLAUDE_PROJECT_DIR`).
- **Docs:** `README.md` (counts + Private-by-default section), `AGENTS.md`, `docs/USING-THIS-TEMPLATE.md`.
- **New ADR-0018** (Gate 4).
- **Honest limit (recorded in ADR):** Tier-0 cannot physically force a fork to stay safe (hooks don't clone; `--no-verify`; an owner can strip it). INV-14 guarantees **safe-by-default + governed + loud-to-remove**, not an unbreakable cage.

---

## Gate 1 — CHECK (Impact Analysis)

**Principle(s) restated (own words):** This is **additive** — it introduces a new Tier-0 invariant rather than overriding one. INV-14 says a deployed vault must not have its private content published outward by automation, and must default to "no push" until the operator deliberately allowlists a destination; public-facing publication needs explicit human confirmation. It does not relax INV-4/5 (write scope) or any other invariant — it **strengthens** the Safety band by closing an exfiltration gap those invariants didn't cover (they govern *writes within* the vault, not *replication outward*). The frozen-ID rule (ADR-0008) is honored: INV-14 is appended; INV-1–13 are untouched.

**Blast radius:**

- [x] `openspec/constitution.md` — §2 Tier-0 Inviolable list gains INV-14
- [x] `openspec/project.md` — Safety band gains INV-14; header range → INV-1–14
- [x] `openspec/specs/access-control/spec.md` — `protects:` += INV-14; ADDED Requirement (INV-14) + scenarios
- [x] `openspec/specs/maintenance/spec.md` — MODIFIED Script Inventory (+ `push-guard-script`)
- [x] `vault-template/99-Operations/scripts/push-guard-script.md` — NEW literate hook (deploy → `pre-push`)
- [x] `vault-template/99-Operations/config.env` + `config.env.example` — `VAULT_PUBLISH_GUARD`, `PUSH_ALLOWLIST` (+ `close-daily`→`daily-close` comment fix)
- [x] `.claude/hooks/outbound-publish-guard.py` + `.claude/settings.json` (repo + vault-template) — portable L1 guard
- [x] `AGENTS.md`, `README.md`, `docs/USING-THIS-TEMPLATE.md` — guidance + counts + Private-by-default
- [x] `vocabulary-lint` — no off-metaphor terms introduced
- [x] _(CHANGELOG / archived changes left intact)_
- [x] ADR-0018 (new)

---

## Gate 2 — PLAN (Migration + Regression)

**Migration plan:**

1. Append INV-14 to `project.md` (Safety band) + `constitution.md` Tier-0 table.
2. `access-control`: add `protects: INV-14` + the ADDED Requirement (delta + canonical frontmatter).
3. `maintenance`: add `push-guard-script` to the Script Inventory (delta).
4. Author the rails: `push-guard-script.md` (deny-by-default, `PUSH_ALLOWLIST`-gated pre-push) + portable `.claude` guard (repo + vault-template) + `config.env`/`.example` keys.
5. Docs (README/AGENTS/USING). ADR-0018.
6. Forks: on `render`, the pre-push deploys deny-by-default; the operator opts in by setting `PUSH_ALLOWLIST`.

**Regression tests that MUST pass before Gate 3:**

- [x] `openspec validate private-by-default-publish-guard --strict`
- [x] `openspec validate --all --strict`
- [x] `constitution-lint` (override acknowledgment + ADR present for the constitution.md / protected-spec diff)
- [x] `vocabulary-lint`
- [x] guard-script unit checks (deny vault-outward; ASK on public publish; allow benign / allowlisted)

---

## Gate 3 — EXECUTE + REGRESSION TEST

**Implementation complete:** ☑ — INV-14 (project.md + constitution.md), access-control (protects + Requirement), maintenance (Script Inventory), `push-guard-script`, portable `.claude` guard (repo + vault-template), config keys (+ v0.1.12 comment fix), docs, ADR-0018
**All regression tests green (local):** ☑ — `openspec validate --all` · constitution-lint · vocabulary-lint · guard unit-tests · residual grep clean
**CI green on this PR:** ☐ (runs on push)

---

## Gate 4 — RE-CHECK + HUMAN SIGN-OFF

**Second review confirms blast radius was fully addressed:** ☑
**Consequences explicitly accepted:**

> Sacrifice: a deployed vault cannot push anywhere until the operator deliberately allowlists a private remote (safety over convenience, inherited by every fork); and INV-14 becomes **self-defending** — removing or weakening it later requires the full Informed-Upheaval ceremony. No existing principle, invariant, or workflow is weakened; the Safety band is strengthened. The guarantee is safe-by-default + governed + loud-to-remove, not a physical impossibility (hooks don't clone; `--no-verify`; an owner can strip it deliberately).

**ADR created:** `openspec/adr/0018-private-by-default-publish-guard.md` ☑
**ADR captures:** context / options / choice / consequence / **sacrifice** ☑

**SIGN-OFF** (human only — agents may not sign):
Name: **Keith Nielsen** — "INV-14 ratified. Full ceremony." (transcribed by Claude Code at Keith's explicit instruction; the decision is the human's)
Date: 2026-06-29
