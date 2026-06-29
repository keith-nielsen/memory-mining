---
type: meta-script
deploy_target: 99-Operations/hooks/pre-push
runtime: git hook
class: script
created: 2026-06-29
updated: 2026-06-29
---
## Rationale
Deny-by-default push-gate for **INV-14** (private by default; no unbid publication). Fires on every
`git push` — by human, script, agent, or sync tool — and **refuses the push unless the target remote
matches `PUSH_ALLOWLIST`** (`config.env`). With `PUSH_ALLOWLIST` empty (the default), every push is
denied: a freshly-deployed vault is private and cannot publish until the operator *deliberately*
allowlists a (private) remote.

Deterministic (INV-6): it reads only the git-supplied remote args and `config.env` — no network, no LLM.
Lives in a tracked folder so it ships with the template; activated per clone via
`git config core.hooksPath 99-Operations/hooks`. `render` must mark it executable (`chmod +x`).

Honest limit: a determined operator can disable `VAULT_PUBLISH_GUARD`, empty-match the allowlist, or
`git push --no-verify`. This is a **safe-by-default + governed** guarantee (INV-14 is Tier-0; removing it
is a constitutional act), not a physical impossibility. The agent-harness `.claude` guard backstops the
agent path.

## Implementation
```bash
#!/usr/bin/env bash
set -euo pipefail

remote_name="${1:-}"
remote_url="${2:-}"

# Pick up config without requiring a pre-sourced shell.
if [[ -n "${VAULT_ROOT:-}" && -f "${VAULT_ROOT}/99-Operations/config.env" ]]; then
    # shellcheck disable=SC1091
    source "${VAULT_ROOT}/99-Operations/config.env"
fi

guard="${VAULT_PUBLISH_GUARD:-on}"      # default ON (deny-by-default) if unset
allow="${PUSH_ALLOWLIST:-}"             # space-separated allowed remote URLs/substrings; empty = none

# Explicit, deliberate opt-out only.
[[ "${guard}" == "off" ]] && exit 0

# Permit only an explicitly allowlisted destination.
for a in ${allow}; do
    [[ -n "${a}" && "${remote_url}" == *"${a}"* ]] && exit 0
done

cat >&2 <<MSG

  ⛔⛔⛔  PUSH BLOCKED — PRIVATE BY DEFAULT (INV-14)  ⛔⛔⛔

  This vault is PRIVATE. Outbound push is denied unless the target remote is
  explicitly listed in PUSH_ALLOWLIST (99-Operations/config.env).

      remote : ${remote_name} ${remote_url}

  To publish DELIBERATELY (private backup only):
    1) confirm the destination is PRIVATE and intended,
    2) add its URL (or a unique substring) to PUSH_ALLOWLIST in config.env,
    3) re-run the push.

  Removing this guard entirely is a constitutional act (INV-14 is Tier-0).

MSG
exit 1
```
