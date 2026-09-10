# 2026-09-10 — T3 Code: Homebrew Cellar Node pinned in LaunchAgent

## Bug
`t3 service install/update` persisted `process.execPath`, which Homebrew realpaths to `/opt/homebrew/Cellar/node/<ver>/bin/node`. After `brew upgrade node` the keg dies and the service fails while `/opt/homebrew/bin/node` still works.

## Fix shipped
Upstream PR: https://github.com/pingdotgg/t3code/pull/11061 (Fixes #11054)

- `stableNodeExecutablePath(execPath, argv0)` prefers durable argv0 (`/opt/homebrew/bin/node`, `opt/node@N`) else rewrites Cellar/Caskroom → `$prefix/bin/<name>`.
- Used in bootService unit write and serviceLauncher handoff spawn.

## Harness learning
Anything that *persists* a runtime binary path must not use the realpath of a package-manager symlink. Same class of bug as pinning Python venv or nvm paths into launchd without a stable shim.

## Review follow-up (`cebee968`)
- Versioned `node@*` → `$prefix/opt/$formula/bin/node` (keg-only).
- Prefer argv0 only when it realpath/inode-matches execPath (blocks `exec -a` spoof).
