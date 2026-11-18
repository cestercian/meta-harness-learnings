# Omnigent

- **Repo:** https://github.com/omnigent-ai/omnigent
- **Role:** Agent harness / control surface with Codex-native (and related) probe homes.
- **Contribution posture (2026-10):** First seam (`#8144`, probe-home credential symlink refresh) was auto-closed 2026-10-02 by `omni-resolve-agent` after the repro agent found #6252 already fixed on main. Re-check that a linked issue is open and still reproduces before and during a PR; stale-linked PRs close without review. `#8312` (session retention) open.
- Probe homes that rewrite `config.toml` each run must also **unlink and rematerialize** credential symlinks (`auth.json`, `.credentials.json`, …) every probe. Leaving existing links (including dangling) after a source Codex home move or recreate leaves probes logged out forever.
- **CI gate (2026-11):** `Maintainer Approval` fails for outside authors until a maintainer approves the *current head SHA*. Pushes we make after approval (merge-main, rebase, update-branch) reset it, and only trusted automation pushes keep an earlier approval. A red gate before review is expected. #8312 was synced with main 2026-11-17 (3103600f) and everything else is green.
- **See also:** `learnings/2026-09-24-t3-multiserver-bridge-browser-omnigent.md`, `learnings/2026-10-05-omnigent-stale-close-openclaw-bot-merge-bridge-release.md`, `learnings/2026-11-18-omnigent-maintainer-approval-sha-pinned.md`
