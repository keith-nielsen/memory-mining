<!-- SPDX-License-Identifier: Apache-2.0 -->
# ADR-0012 — Daily close lifecycle; the Logbook as meta-retention corpus

**Status:** Accepted
**Date:** 2026-06-17
**Relates:** `maintenance` spec (Daily Close Lifecycle) · `close-daily` runbook · change `spec-as-code-runbooks`

## Context

The daily lifecycle *opened* days (`daily-note` + `rollover`) but never *closed* them. Loose
Intentions and Captured nuggets could silently vanish. We also wanted to retain even
"passed-over" material for future pattern-mining — without re-creating the hoard the system
exists to prevent.

## Decision

- A daily MUST pass a deterministic **close ritual**: assign every item exactly one disposition
  from the controlled set (`claim · site · crucible · banked · slagged · spoiled · realized ·
  recorded · passover`) in an appended `## Close` manifest, then set `closed:`.
- Invariants: **append-only** (items never edited/removed), **total-disposition**, **strict-order
  close**, and **gated advance** (capture-home always exists; only rollover/carry-over is gated on
  the prior close).
- **The Logbook is the meta-retention corpus** — append-only, now Bitcoin-sealable. `passover`
  items stay inline as feedstock. Object-Tailings remains for re-minable *ore* only; the daily is
  NOT flooded with non-ore.
- The future **Historical Crucible** — an independent retrospective meta-analysis over the Logbook
  that rescues value the BAU pipeline missed — is the consumer. Deferred; recorded here.

## Options considered

1. **Route all trash to Tailings** — rejected: erodes the Tailings/Spoil anti-bloat distinction (CONST-01).
2. **Hard-gate day *creation* on prior close** — rejected: breaks the guaranteed capture-home invariant.
3. **Logbook-as-corpus + gated *advance* (chosen)** — retains everything without bloat, keeps capture frictionless.

## Consequences

- Nothing un-actioned silently vanishes; the daily archive is a complete, mineable record.
- **Sacrifice:** the discipline of closing each content-bearing day before advancing (empty days
  auto-close). The `passover` disposition explicitly retains "saw it, not now" without judgment.
