# Hermes Agent (NousResearch)

- **Repo:** https://github.com/NousResearch/hermes-agent
- **Role:** Agent product with desktop/web surfaces, custom model-provider plugins, and a `uv`-backed package-manager bridge (`pm/`).
- **Contribution posture (2026-09):** Active review from Enough1122-style probes; expect blockers that measure the wire and Windows CI, not only green Linux tests.

## Notes for contributors

- Custom / OpenAI-compat Claude routes (e.g. CometAPI → Bedrock): Opus 5.5 needs `thinking.type=adaptive` + `output_config.effort`, not top-level `reasoning_effort` → `thinking.type=enabled`. Mandatory-thinking ids omit disable instead of sending `thinking.disabled` (`#123108`).
- When sharing Anthropic adaptive helpers onto the OpenAI-compat path: gate early returns on a **non-empty** adaptive payload (haiku falls through to `reasoning_effort`); add every emitted key (`output_config`) to strip-retry sets; keep thinking-off floor/memoisation reachable.
- Lock verification: ambient `UV_DEFAULT_INDEX` / `UV_INDEX_URL` must not steal `uv lock --check`. Feed registries from `uv.lock` as `--index`; do not only `env.pop` (breaks mirror-only / air-gapped indexes). Windows fakes need `.bat` shims for `CreateProcess` (`#123116`).
- Desktop package.json: declare imports used by onboarding cards (e.g. `lucide-react`); avoid lockfile churn from regenerating on the wrong npm major (`#123053`).

## See also

- `learnings/2026-09-28-hermes-wire-bridge-codex-openclaw-prove.md`
- `learnings/2026-09-30-bridge-dev-updater-mcp-locale-atlas.md`
