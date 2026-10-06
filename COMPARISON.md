# Comparison snapshot (2026-09)

| Dimension | OpenClaw | Prime Agent | T3 Code | OrcaReplay | OrcaCode Review | Bridge |
| --- | --- | --- | --- | --- | --- | --- |
| Primary job | Always-on multi-channel assistant | Long-horizon coding/research agent | Control surface over agent CLIs | Capture & time-travel agent runs | Automated PR review | Native macOS control room for agent CLIs |
| Session model | Durable product sessions | Daemon + worker + IPython RLM | Server owns sessions; clients over WS | Recorded trajectory / fork points | Per-PR / per-push review runs | Task workspaces + durable chats |
| Tools | Channel + product tools | Programmatic tools via REPL | Delegates to provider CLIs | Intercepts / records tool+model I/O (+ origin, vision, agent structure, LangGraph nodes) | Review engine + gateway | Delegates to provider CLIs; approval UX; native usage menu (serving-model ledger); degraded replay carriers; loaded-only MCP context cost |
| Memory / skills | Product memory; ClawSweeper automation | Continual Harness (ρ,G,K,M) + `/refine` | Provider-native + orchestration | Replay artifacts | Rubric / policy in router | Project prefs / knowledge (product) |
| Multi-agent | Teams / channels | `rlm.spawn` children; one `list_agents` roster | Many providers, thread-per-branch | Fork onto other models | Cheap then strong model cascade | Side-by-side providers / fleet |
| UI / control | Product UX + bots | Agents View / ACP | Web, desktop, mobile | CLI (`orca record` …) | GitHub Action + console | Native macOS (usage menu, panes) |
| Contrib posture | PRs + ClawSweeper (prove-on-main / after-fix; idle≠keepalive) | Discussions first; vouched + ENG/RES Linear | Active triage (`accepted`); UI needs before/after evidence (Macroscope≠merge) | Fail-loud flags/scrub; vision + agent-structure capture | Fail-closed merge gates; one authoritative run for reactions/clean verdicts | Active; near-daily Release Please releases; topology ≠ credential policy (fail closed) |

Update this table when a contribution proves a cell wrong.

## Also in orbit (2026-09-17)

- **NeMo Agent Toolkit** — NVIDIA multi-agent toolkit (retries, tools, LangChain/ADK adapters). Detail: `harnesses/nemo-agent-toolkit.md`.
- **OpenCode** — anomalyco/opencode coding agent. Detail: `harnesses/opencode.md`.
- **NemoClaw** — NVIDIA agents-in-OpenShell (5 open PR cap; local routes prefer direct tool disclosure; backup restore scopes by `.sandbox` marker). Detail: `harnesses/nemoclaw.md`.
- **Omnigent** — omnigent-ai/omnigent (Codex-native probe homes; refresh credential symlinks each probe; repro agent + `omni-resolve-agent` auto-close PRs whose linked issue was already fixed). Detail: `harnesses/omnigent.md`.
- **Hermes Agent** — NousResearch/hermes-agent (custom OpenAI-compat thinking wire; uv lock checks pin lockfile registries). Detail: `harnesses/hermes-agent.md`.
- **JevHarness** — TianyuCodings/JevHarness (Windows-portable file locks for JevClient import; `#2` open).
- **foreman** — VisionForge-OU/foreman (skill changelog reviewer identity; `#24` open).
- **ATLAS** — inferstep/ATLAS (adaptive test-time learning / eval driver; absolutize suite roots before Docker grader mounts; `#276` merged).
- **mcp-memory-service** — doobidoo/mcp-memory-service (agent memory MCP; `MCP_LOCALE` must win over harvest aliases; do not freeze locale behind import-time caches; `#1382` merged).
- **HiveGate** — hivegate-ai/hivegate (Agno-based agent runtime; docs hygiene on env defaults; `#76` open).
- **Sotto** — getsotto/sotto (secrets TUI; DCO `Signed-off-by` on every commit; `#473`/`#474` open).