# T3 Code

- **Repo:** https://github.com/pingdotgg/t3code
- **Site:** https://t3.codes/
- **Role:** Open-source control plane for coding agents (Claude Code, Codex, Cursor, Grok, OpenCode, …).
- **Shape:** Node server + clients (web/desktop/mobile); providers as drivers; orchestration over WS.
- **Contribution posture:** Active bug triage with labels like `accepted` / `via-triage`. Fast merge when scoped **and** evidence matches [CONTRIBUTING verification](https://github.com/pingdotgg/t3code/blob/main/CONTRIBUTING.md#verification). UI PRs need before/after screenshots (recordings when motion matters) attached on the PR — Macroscope approve alone does not stop verification closure (`#12890` / `#12906` closed 2026-10-01). Prefer the documented fix path triage already named (e.g. `$T3CODE_HOME/service.env` over preserving unknown unit/plist `Environment=` keys — see #12633 / #12626). **Disclose model and harness in the PR body** — stale PRs (>~2 weeks) without that metadata were mass-closed on 2026-10-11 by `maria-rcks` (`#12633`/`#12863`/`#12878`/`#12889`); rebuild on current `main` with a newer model if the change still matters. Aging PRs usually land as **maintainer takeovers** (`juliusmarminge`): contributor commits kept, main merged, extras and tests trimmed, and the original approach credited even from a closed PR (`#19733` credits our `#12889`). Deferring to another PR ("#N already covers this") rarely holds: 1 of 4 held (`#12768` for `#12767`, the reporter's own same-day PR), while a later PR landed in the other 3 (`learnings/2026-10-20-*`, `learnings/2026-10-21-*`). Credit is not automatic either: our closed `#12633` service.env design landed as `#19486` credited only to its author, so re-open a disclosure-closed PR quickly if it still matters.

## Notes for contributors

- Desktop annotation under CSP: decode `data:` PNGs locally (`dataUrlToFile`); do not `fetch(dataUrl)` (#12636 / #12265).
- Linux SnapShot a11y: Flatpak PID lookup fails through `xdg-dbus-proxy`; GTK4 may return unnamed groups — title+size / unique bounds fallbacks (#12635 / #12597).
- ACP drivers: keep redacted stderr on process exit; do not collapse to opaque session-closed (#12625 / #12451).
- Claude `homePath`: empty / `~/.claude` / absolute must share one continuation key like the CLI (#12624 / #12616).
- Pending questions: park typed custom answers onto the thread draft when an option is clicked (#12577 / #12569).
- Windows `.cmd`/`.bat`: single command string + empty args under `shell: true` to avoid DEP0190 (#12863 / #12797). Confirmed: maintainer shipped this shape in `#19385` (merged 2026-10-14) with cross-spawn byte-match proof; see `learnings/2026-10-15-*`.
- Antigravity wrappers: unwrap to real ACP `.exe` + matching `localharness_external`; do not hang on `cmd.exe /c` (#12878 / #12752). 2026-10-22 triage: availability no longer starts ACP (`8c18b5bb21`), and borrowing a managed-release helper is a version-compat decision for maintainers, so require the helper beside the resolved `.exe` and error otherwise (`learnings/2026-10-22-*`).
- Codex app-server: per-message byte ceiling on JSONL fragments, not only queue depth (#12889 / #12884). Landed as `#19733` (2026-10-20): 128 MiB UTF-8 cap, LF excluded, end the transport before join/log/parse.
- Codex app-server fragment ceiling: count UTF-8 bytes (`TextEncoder`), not UTF-16 code units after `decodeText` (#12889).
- Provider update icons: identical glyphs must open the same Update popover; copy stays a distinct control (#12890 closed for missing UI evidence / #12886).
- Diff file tree: file↔dir prefix collision → flat list fallback (#12906 closed for missing UI evidence / #12887). Also reproduces dir→symlink (`120000`) on nightly 2026-10-14, incl. GitLab MRs. Landed as `#18133` (2026-10-19) with a different shape: alias only the colliding files and map selection and reveal back to the real paths, not a flat list.
- Multi-server shared state dir: reconcile projects recovered events locally but only republishes events this dispatch appended — never peer `thread.turn-start-requested` (#13295 merged 2026-10-02 / #13275).
