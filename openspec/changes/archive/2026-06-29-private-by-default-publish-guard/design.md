## Context

A deployed vault is private and holds irreversible-if-leaked material, but nothing structural stops an
automated actor (or a fatigued human led by one) from publishing it outward. The incident that
triggered this: an agent *suggested*, as a convenience, pushing the private vault to a public repo
(captured in the live vault as the `safety-rail-requirements` Claim). Instructions alone fail exactly
when judgment is tired — the fix must be structural and self-defending.

## Goals / Non-Goals

**Goals:** make private-by-default a Tier-0 invariant (INV-14); ship a forkable, deny-by-default,
allowlist-gated push-guard + a portable agent-harness guard; document the stance.

**Non-Goals:** sandboxing the OS; preventing a determined operator from deliberately stripping the guard
(impossible and out of scope); gating the public *template repo*'s own pushes (it is public by design —
the guard only activates inside a deployed vault, keyed on `VAULT_ROOT`).

## Decisions

- **Tier-0, not a convention.** Ratified by the operator: the protection must be self-defending —
  removing/weakening INV-14 itself requires the Informed-Upheaval ceremony. Append per ADR-0008
  (frozen IDs); home = `access-control` (the safety-invariant spec).
- **Deny-by-default + allowlist.** `PUSH_ALLOWLIST` empty ⇒ refuse all pushes. The operator opts in by
  listing a (private) remote. This is safe-by-default *and* adoptable.
- **Two enforcement layers.** (1) `pre-push` hook (deterministic, INV-6) — catches any pusher, including
  a human terminal push (modulo `--no-verify`). (2) `.claude` `PreToolUse` guard — catches the agent
  before execution and binds its *mouth* (loud ASK on public publish; hard-deny vault-outward). Neither
  alone is sufficient; together with "no remote by default" (L3) it is defense-in-depth.
- **Portable, not machine-specific.** The guard derives the protected path from `$VAULT_ROOT` (else
  `$CLAUDE_PROJECT_DIR`); the public-publish ASK is universal. So it works in any fork without edits.
- **Template repo unaffected.** It has no `VAULT_ROOT`, so the vault-outward deny is inert there; only
  the public-publish ASK applies (appropriate — releases/repo-creation should be deliberate anyway).

## Risks / Trade-offs

- **Cannot physically force a fork to stay safe** — hooks don't clone, `--no-verify` exists, an owner can
  delete the hook. Mitigated by: shipping deny-by-default in the rendered hook (active after the same
  `render`/`core.hooksPath` step the commit-gate already needs), the harness guard, loud docs, and the
  Tier-0 governance making removal a conscious, ceremonied act. Stated honestly in ADR-0018.
- **Friction:** a fresh vault can't push until the owner allowlists. Intended.

## Migration Plan

Append INV-14 (`project.md` + `constitution.md`) → `access-control` (protects + Requirement) →
`maintenance` (Script Inventory) → author rails (`push-guard-script`, portable `.claude` guard, config
keys) → docs → ADR-0018 → Gate-4 → archive (sync specs) + CHANGELOG `[0.1.13]` + tag `v0.1.13` → mirror
to live vault (deploy `pre-push`, set `PUSH_ALLOWLIST=` empty, retire the hand-rolled local hook).

## Open Questions

- None blocking. OS-level egress controls remain a deeper, separate (deferred) hardening.
