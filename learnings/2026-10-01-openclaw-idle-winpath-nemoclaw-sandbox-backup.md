# 2026-10-01 — OpenClaw idle vs progress, Windows long paths, NemoClaw sandbox-scoped restore

Activity after the 2026-09-30 Bridge/MCP/ATLAS note (commit `7e9fc2f`). Late 09-30 opens on OpenClaw and NemoClaw carry reusable failure patterns; ClawSweeper again blocked on after-fix proof.

## OpenClaw — empty stream chunks must not re-arm the LLM idle watchdog (`#162018` open)

OpenAI-compatible providers can keep a completions stream alive with content-free chunks (`"choices": []` or empty `delta`) after response headers. The transport reported request activity for **every** parsed chunk, so keepalive-style traffic re-armed the idle watchdog forever. The turn then hung until the full run timeout (~600 s) with an empty reply and no same-model retry, instead of tripping the normal ~120 s LLM idle path.

**Fix posture:** report activity only for chunks that carry progress (non-empty delta content, hidden reasoning, tool calls, finish reason, or usage). Empty-choice keepalives leave the idle timer running.

ClawSweeper blocked merge readiness and raised a contract fork that will recur on other agent runtimes:

| Question | ClawSweeper lean |
| --- | --- |
| Idle = connection liveness, or model progress? | **Separate them:** keep a transport-liveness signal; add a bounded **model-progress** deadline for empty-chunk stalls |
| Synthetic unit tests enough? | No — wants a redacted after-fix fake-server agent trace showing idle abort + same-model retry |

Compatibility risk: providers that send empty JSON chunks during a legitimate long silent phase would now hit idle sooner. Document the tradeoff; do not silently redefine "chunk arrived" as "model made progress."

## OpenClaw — strip Windows `\\?\` before path set membership (`#162033` open)

On Windows, every durable `sessions.create` failed with "Session creation publication owner is no longer current." SQLite reports the agent database as a Win32 extended-length path (`\\?\C:\...`), while the creation-publication guard compared against a set of plain `path.resolve` strings. Membership always failed even when the DB was the selected store.

**Remedy:** normalize with the existing `normalizeWindowsPathPrefix` (strip the extended-length prefix) on the source path before comparing. Keep rejecting creations whose database is outside the selected storage targets.

ClawSweeper still wants after-fix proof from a **real Windows Gateway** (`sessions.create` → usable session), not only Linux tests fed a Windows-spelled string. Same prove-on-platform bar as the 09-28 prove-on-main culture.

## NemoClaw — restore-without-timestamp must scope by sandbox identity (`#12539` open)

`scripts/backup-workspace.sh restore <sandbox>` with no timestamp used to pick the newest backup under `~/.nemoclaw/backups/<timestamp>/` with **no record of which sandbox** produced it. With two sandboxes, `restore alice` could overwrite alice's `SOUL.md` / `USER.md` / `IDENTITY.md` / `MEMORY.md` / `memory/` with bob's and exit 0.

| Change | Detail |
| --- | --- |
| Backup | Write a `.sandbox` marker naming the source sandbox |
| Restore (no timestamp) | Only consider backups whose marker matches; fail loud if none match |
| Restore (explicit timestamp) | Still allowed across sandboxes; warn when the marker disagrees |
| Legacy backups | No marker → require an explicit timestamp (do not guess) |

Extends the existing fail-loud backup posture (`#12108` permission-denied dirs) from "do not claim a full archive" to "do not restore the wrong tenant's archive."

## Status only (no new architecture lesson)

- T3 `#13295` — Macroscope re-approved 2026-10-01; multi-server reconcile scope already in `learnings/2026-09-24-t3-multiserver-bridge-browser-omnigent.md`.
- OrcaRouter-Lite `#145` merged (BYOK format warnings) — covered 09-24 / 09-25.
- mcp-memory-service `#1382` merged — covered 09-30.
- foreman `#25` open — finalize dangling `running` workers when a build/resume ends (e2e agent never calls `worker_finished`); thin lifecycle hygiene, babysit.
- Bridge `#745` / `#747`, hivegate `#76`, Hermes `#123108` / `#123116`, JevHarness `#2`, Omnigent retention: quiet or already noted. Prime still vouch-gated.

## Carry-forward

- Stream idle watchdogs: key re-arms on **model progress**, not every parsed chunk; consider separate liveness vs progress deadlines.
- OpenClaw / ClawSweeper: after-fix (or after-fix-on-Windows) proof still gates merge; synthetic path-spelling tests are not enough for platform bugs.
- Windows path guards: always normalize `\\?\` / extended-length forms before set membership or equality with plain resolved paths.
- Multi-sandbox backups: persist tenant/sandbox identity in the archive; default restore without an explicit id must fail closed, not pick global newest.
- Prior 09-28 prove-on-main and 09-30 Bridge updater / locale / ATLAS carry-forwards still stand.

## Links

- https://github.com/openclaw/openclaw/pull/162018
- https://github.com/openclaw/openclaw/pull/162033
- https://github.com/NVIDIA/NemoClaw/pull/12539
- https://github.com/VisionForge-OU/foreman/pull/25
- https://github.com/pingdotgg/t3code/pull/13295
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/145
