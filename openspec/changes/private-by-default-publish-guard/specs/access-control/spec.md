## ADDED Requirements

### Requirement: Private by Default — No Unbid Publication (INV-14)

A deployed vault SHALL be private by default. No automated actor (Agent or Script) may push,
mirror, or otherwise replicate vault content to any remote or external/third-party destination,
**except** a destination the operator has explicitly listed in `PUSH_ALLOWLIST` (`config.env`).
With `PUSH_ALLOWLIST` empty (the default), every outbound push is refused.

Creating a public repository, or publishing to an external distribution hub, from within the vault
SHALL require explicit, deliberate **human** confirmation — and SHALL never be initiated as an
agent's unprompted suggestion.

Enforcement is **structural**, never trust-based:
- a deterministic `pre-push` hook (`push-guard-script`, INV-6) that denies by default and permits only
  an allowlisted remote; and
- the agent harness guard (`.claude` `PreToolUse`) that hard-denies vault-outward commands and raises a
  loud, explicit confirmation before any public-facing publication.

This invariant is **additive** to the Safety band and weakens nothing: INV-4/5 bound *writes within*
the vault; INV-14 bounds *replication outward*. INV-14 is appended per the frozen-ID rule (ADR-0008);
INV-1–13 are unchanged.

#### Scenario: Automated push to a non-allowlisted remote is denied
- **WHEN** an Agent or Script triggers `git push` from the vault to a remote not in `PUSH_ALLOWLIST`
- **THEN** the `pre-push` hook aborts with an INV-14 violation; nothing is transmitted

#### Scenario: Agent must not propose outbound publication
- **WHEN** a task could be "helped" by pushing/mirroring vault content outward or creating a public repo
- **THEN** the agent does not suggest or perform it; the harness `PreToolUse` guard denies the vault-outward command and requires deliberate human action for any public publication

#### Scenario: Operator opt-in is explicit and deliberate
- **WHEN** the operator wants an off-machine backup
- **THEN** it is permitted only after the operator deliberately adds that (private) remote to `PUSH_ALLOWLIST`; a tired or quick assent solicited by an agent does not satisfy this
