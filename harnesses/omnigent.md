# Omnigent

- **Repo:** https://github.com/omnigent-ai/omnigent
- **Role:** Agent harness / control surface with Codex-native (and related) probe homes.
- **Contribution posture (2026-09):** First seam claimed — probe-home credential symlink refresh (`#8144` open / Fixes #6252).
- Probe homes that rewrite `config.toml` each run must also **unlink and rematerialize** credential symlinks (`auth.json`, `.credentials.json`, …) every probe. Leaving existing links (including dangling) after a source Codex home move or recreate leaves probes logged out forever.
- **See also:** `learnings/2026-09-24-t3-multiserver-bridge-browser-omnigent.md`