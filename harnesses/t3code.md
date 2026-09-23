# T3 Code

- **Repo:** https://github.com/pingdotgg/t3code
- **Site:** https://t3.codes/
- **Role:** Open-source control plane for coding agents (Claude Code, Codex, Cursor, Grok, OpenCode, …).
- **Shape:** Node server + clients (web/desktop/mobile); providers as drivers; orchestration over WS.
- **Contribution posture:** Active bug triage with labels like `accepted` / `via-triage`. Fast merge when scoped. Prefer the documented fix path triage already named (e.g. `$T3CODE_HOME/service.env` over preserving unknown unit/plist `Environment=` keys — see #12633 / #12626).

## Notes for contributors

- Desktop annotation under CSP: decode `data:` PNGs locally (`dataUrlToFile`); do not `fetch(dataUrl)` (#12636 / #12265).
- Linux SnapShot a11y: Flatpak PID lookup fails through `xdg-dbus-proxy`; GTK4 may return unnamed groups — title+size / unique bounds fallbacks (#12635 / #12597).
- ACP drivers: keep redacted stderr on process exit; do not collapse to opaque session-closed (#12625 / #12451).
- Claude `homePath`: empty / `~/.claude` / absolute must share one continuation key like the CLI (#12624 / #12616).
- Pending questions: park typed custom answers onto the thread draft when an option is clicked (#12577 / #12569).
- Windows `.cmd`/`.bat`: single command string + empty args under `shell: true` to avoid DEP0190 (#12863 / #12797).
- Antigravity wrappers: unwrap to real ACP `.exe` + matching `localharness_external`; do not hang on `cmd.exe /c` (#12878 / #12752).
- Codex app-server: per-message byte ceiling on JSONL fragments, not only queue depth (#12889 / #12884).
- Codex app-server fragment ceiling: count UTF-8 bytes (`TextEncoder`), not UTF-16 code units after `decodeText` (#12889).
- Provider update icons: identical glyphs must open the same Update popover; copy stays a distinct control (#12890 / #12886).
- Diff file tree: file↔dir prefix collision → flat list fallback (#12906 / #12887).
