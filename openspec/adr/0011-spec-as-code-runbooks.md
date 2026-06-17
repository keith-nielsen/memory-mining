<!-- SPDX-License-Identifier: Apache-2.0 -->
# ADR-0011 — Spec-as-code runbooks: agnostic, deterministic operational procedures

**Status:** Accepted
**Date:** 2026-06-17
**Relates:** `maintenance` + `vault-structure` specs · `96-Runbooks` band · change `spec-as-code-runbooks`

## Context

Repeatable, error-prone operations (closing a daily, sealing provenance) lived only in
conversation and prose-memory. Across a context clear or a model/harness swap (Opus↔DSv4,
Claude Code↔Hermes), agents re-improvised them and hallucinated variations. Operational
knowledge needs to be reproducible *without* context.

## Decision

- Runbooks are **spec-as-code**: literate, schema-validated procedure notes in a dedicated
  `96-Runbooks/` band (validated by `runbook-lint`) — the single source of truth.
- The canonical substrate is **model- and harness-agnostic** plain Markdown + deterministic
  scripts. Harness files (`CLAUDE.md`, `AGENTS.md`, Claude Code skills) are **adapters** that
  point at the runbook; they never hold the source of truth.
- **Determinism first:** push every step into `.py`/`.sh` (INV-6); reserve AI for genuine
  interpretation, bounded to an `unknown/other` fallback over an enumerated state list, with a
  harvest loop that promotes recurring resolutions into deterministic rules (AI surface → 0).

## Options considered

1. **Claude Code Skills as the home** — rejected: tool-specific, violates the harness-agnostic goal.
2. **Prose docs in `docs/`** — rejected: drifts, not schema-validated, not a first-class band.
3. **Spec-as-code runbooks in a vault band (chosen)** — agnostic, validated, forkable, and the
   most deterministic substrate (a `.py` step runs identically on any model/harness).

## Consequences

- One more infra band per vault; forks `mkdir 96-Runbooks/` on upgrade (the reserved `90–96`
  range narrows to `90–95`). CONST-04/CONST-02 are upheld, not overridden.
- Operational procedures survive context clears and model/harness swaps by construction.
- **Sacrifice:** a little structural surface, plus the discipline of keeping adapters thin. The
  hard guarantee remains the mechanical gates (CI/hooks), not the prose.
