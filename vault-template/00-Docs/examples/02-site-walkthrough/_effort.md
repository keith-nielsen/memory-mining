---
type: effort
title: git bisect for regression hunting
status: ore
grade: gold
pillars: [technology]
started: 2026-05-29
completed: 
slag_reason: 
---
# Effort: git bisect for regression hunting

## Goal / definition of done

Understand `git bisect` well enough to use it reflexively the next time a regression
appears. Done when I can: (a) run a bisect start-to-finish, (b) automate it with a
test script, and (c) state the one durable principle worth keeping.

<!--
  This effort has reached status: ore. It was promoted from the Claim, walked
  through prospect → dig → ore, and its grade was ESTIMATED as gold. The messy
  working notes below are the Dig — they are NOT the durable output. The durable
  output is the distilled bullion (see 04-treasury-bullion.md). Dig notes are
  allowed to be long, redundant, and exploratory. That is their job.
-->

## Dig (working notes)

Tried it on the broken nightly build. Known-good tag `v2.3.0`, known-bad `HEAD`.

```
git bisect start
git bisect bad HEAD
git bisect good v2.3.0
# git checks out a commit halfway between; I build + test, then mark:
git bisect good   # or: git bisect bad
# repeat — each answer halves the remaining range
git bisect reset  # when done, returns to original HEAD
```

It found the culprit in 6 steps across ~60 commits. By hand that was 3 hours of
linear checkout-build-test; bisect was ~20 minutes. The math is just log2(N):
60 commits → ~6 tests instead of up to 60.

### Automating it
The real unlock: `git bisect run <cmd>`. Give it a script that exits 0 for good,
non-zero for bad, and bisect drives itself to the first bad commit with zero manual
marking.

```
git bisect start HEAD v2.3.0
git bisect run ./scripts/check-regression.sh
```

Gotchas I hit:
- The test script must be RELIABLE. A flaky test makes bisect converge on the wrong
  commit. Spent 15 min chasing a false result before realizing the test itself was
  intermittent.
- Use exit code 125 to tell bisect "skip this commit, can't test it" (e.g. it
  doesn't even build for unrelated reasons).
- Bisect assumes monotonicity: good stays good until the one bad commit. If the bug
  appears and disappears, bisect's binary-search assumption breaks.

### Side reading
- `git bisect terms` lets you rename good/bad (e.g. old/new) for non-bug searches —
  you can bisect on ANY binary property, not just "broken." Found a perf regression
  this way by scripting a benchmark threshold.

## Decisions
<!-- approaches tried, what was rejected and why -->

- **Estimated grade: gold.** Not because it took effort (it didn't), but because it
  permanently changes my approach to every future regression. Value, not effort.
- Considered filing this under a "debugging" sub-area, but pillars are not folders —
  it gets `pillars: [technology]` and a Catalog link instead (INV-12).
- The durable insight is NOT "here are the bisect commands" (those are in `man git-bisect`).
  The durable insight is the *reframe*: regression hunting is a binary search, and
  `bisect run` makes it automatic. That reframe is what goes to the Treasury.
