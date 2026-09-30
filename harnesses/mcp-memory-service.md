# mcp-memory-service

- **Repo:** https://github.com/doobidoo/mcp-memory-service
- **Role:** Persistent memory MCP / REST for agent pipelines (harvest, knowledge graph, bootstrap formatters).
- **Contribution posture (2026-09):** Active; expect greptile on env/cache seams.

## Notes for contributors

- `MCP_LOCALE` is the public switch; harvest aliases like `HARVEST_LOCALE` are fallbacks only (`#1382`).
- Do not `@lru_cache` the **resolved** locale for the process lifetime — import-time callers (e.g. NLI) freeze the first read. Re-read env each call; cache parsing only.
- Bootstrap formatters: store factories, construct when requested, so a locale set after import reaches Kiro and friends.

## See also

- `learnings/2026-09-30-bridge-dev-updater-mcp-locale-atlas.md`
