---
type: runbook
id: close-daily
title: Close a daily note (full disposition sweep)
trigger: "the date has rolled over and a prior daily is still open; close it before advancing"
applies-to: vault
class: procedure
last-validated: 2026-06-17
---
# Runbook — Close Daily

## Purpose
Bring a daily note to a fully-dispositioned, append-only `closed` state so nothing un-actioned
silently vanishes, the Logbook becomes a complete meta-corpus, and the next day may open.

## Preconditions
- `[script]` `vault-close-day.py` rendered to `~/bin`; `VAULT_ROOT` set; the vault venv on `PATH`.
- Strict order: every daily before the target is already `closed` (the script refuses otherwise).

## Steps
1. `[script]` `vault-close-day.py <date>` parses the day and classifies every item by rule, in
   priority order: already-resolved → wikilink-target lookup → section default (`Log`/`Decisions`
   = `recorded`) → `Idea:` line → `claim` → trailing `?` → flagged → else `unknown/other`.
2. `[agent]` Resolve **only** the emitted `unknown/other` worklist — one disposition each from
   `claim · site · crucible · banked · slagged · spoiled · realized · recorded · passover`. This is
   the **only** AI step. Perform object-actions: nuggets → `20-Claims`, insights → `30-Sites` (dig),
   etc. Record resolutions in the sidecar (items themselves stay untouched — append-only).
3. `[script]` re-run `vault-close-day.py <date>` → appends the `## Close` manifest, sets
   `closed: <today>`, one commit. Then `vault-close-day.py --check <date>` asserts the invariants.
4. `[script]` open the next day + carry over: `vault-daily-note.py` then `vault-rollover.py`
   (rollover refuses while the prior day is unclosed; the capture stub always exists).

## Pitfalls
- **Append-only:** never edit items above `## Close`; dispositions live in the manifest / sidecar, not inline.
- Parked `ore` does not carry over (the kanban / refine-queue is its home) — by design, not a leak.
- The script never calls a model (INV-6); the `[agent]` resolution happens outside it.
- **Harvest:** when an `unknown/other` recurs, add a deterministic rule so the AI surface shrinks toward zero.

## Verification
- `vault-close-day.py --check <date>` exits 0: `closed:` set, total-disposition, vocab ∈ enum, append-only intact.
- The next day's note exists; its `## Carry-over` lists only open `dig` Sites.

## Rollback
- Before commit: `git checkout -- <path-to-day>.md` restores the pre-close state.
- After commit: the close is one atomic commit — `git revert` it to reopen the day, then re-run.
