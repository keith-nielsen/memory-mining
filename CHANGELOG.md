<!-- SPDX-License-Identifier: Apache-2.0 -->
# Changelog

This changelog is generated from completed OpenSpec changes in
`openspec/changes/archive/`. Each entry corresponds to an archived change.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [Unreleased]

<!-- New entries are added here as changes land. -->

---

## [0.1.0] - 2026-06-15

First validated release. The deterministic engine (Phases 0–2) is proven against
the full PRD acceptance suite; Phase 3 (agent operations) remains spec-only/deferred.

### Added
- Initial repository structure: OpenSpec SDD scaffold, vault-template skeleton,
  constitution, 6 capability specs, 8 ADRs, 2 archived teaching changes,
  1 live change stub (add-telemetry-segment), CI pipeline, docs layer.
- Worked end-to-end example in `vault-template/00-Docs/examples/` (Claim → Treasury).
- `.github/scripts/validate-scripts.sh` — renders all 13 meta-scripts and runs
  `py_compile` + `shellcheck` + a fresh-vault pipeline smoke + the INV-11 executor
  boundary test. Wired as a CI matrix job (Python 3.12, 3.13).

### Fixed
- `config.env` used an HTML comment (`<!-- SPDX -->`) on line 1, which broke
  `source 99-Operations/config.env`. Changed to a shell comment (`# SPDX`).
- The literate-script render extractor used a non-line-anchored regex that
  truncated any script whose body contains a triple-backtick (notably
  `render-reconcile` itself). Anchored the closing fence to line start
  (`^``` ` with `re.MULTILINE`) in the script and both documented bootstrap
  snippets (`README.md`, `docs/USING-THIS-TEMPLATE.md`).
- The in-vault bootstrap (`00-Docs/README.md`) instructed running a `.md`
  meta-script note directly as Python; replaced with the code-block extraction step.
- Cron and ongoing-ops invocations set only `VAULT_ROOT`, so `vault-refine-detect.py`
  (needs `REFINE_GATE_GRADES`) and `vault-lint.py` (needs `PILLARS`/`GRADES`/
  `KNOWLEDGE_STAGES`) would `KeyError`. All documented invocations now source
  `config.env`.

### Changed
- Aligned proposal-schema MOC path examples with the kebab-case filenames
  (`<pillar>-moc.md`) used by the actual template (INV-11).
- Supported Python floor set to **3.12+** (was advertised as 3.10+, which the
  version matrix showed was not actually met).

### Validated
- Full PRD Phase 0→2 acceptance suite (A0.1–A2.6, plus orphan detector) against a
  sandboxed vault: 19/19 checks pass. All 13 operational scripts deploy via
  `render`, `reconcile` reports zero drift, and the refine pipeline
  (detect → propose → gate → execute), dispose, slag, rollover, kanban, linter,
  naming validator, and commit-gate hook all behave per spec.
- The documented onboarding was dogfooded literally end-to-end on a fresh vault.

[0.1.0]: https://github.com/keith-nielsen/memory-mining/releases/tag/v0.1.0
