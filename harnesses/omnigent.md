# Omnigent

- **Repo:** https://github.com/omnigent-ai/omnigent
- **Role:** Agent harness / control surface with Codex-native (and related) probe homes.
- **Contribution posture (2026-10):** First seam (`#8144`, probe-home credential symlink refresh) was auto-closed 2026-10-02 by `omni-resolve-agent` after the repro agent found #6252 already fixed on main. Re-check that a linked issue is open and still reproduces before and during a PR; stale-linked PRs close without review. `#8312` (session retention) open.
- Probe homes that rewrite `config.toml` each run must also **unlink and rematerialize** credential symlinks (`auth.json`, `.credentials.json`, …) every probe. Leaving existing links (including dangling) after a source Codex home move or recreate leaves probes logged out forever.
- **See also:** `learnings/2026-09-24-t3-multiserver-bridge-browser-omnigent.md`, `learnings/2026-10-05-omnigent-stale-close-openclaw-bot-merge-bridge-release.md`