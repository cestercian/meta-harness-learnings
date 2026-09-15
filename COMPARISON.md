# Comparison snapshot (2026-09)

| Dimension | OpenClaw | Prime Agent | T3 Code | OrcaReplay | OrcaCode Review | Bridge |
| --- | --- | --- | --- | --- | --- | --- |
| Primary job | Always-on multi-channel assistant | Long-horizon coding/research agent | Control surface over agent CLIs | Capture & time-travel agent runs | Automated PR review | Native macOS control room for agent CLIs |
| Session model | Durable product sessions | Daemon + worker + IPython RLM | Server owns sessions; clients over WS | Recorded trajectory / fork points | Per-PR / per-push review runs | Task workspaces + durable chats |
| Tools | Channel + product tools | Programmatic tools via REPL | Delegates to provider CLIs | Intercepts / records tool+model I/O (+ origin, vision, agent structure) | Review engine + gateway | Delegates to provider CLIs; approval UX; native usage menu |
| Memory / skills | Product memory; ClawSweeper automation | Continual Harness (ρ,G,K,M) + `/refine` | Provider-native + orchestration | Replay artifacts | Rubric / policy in router | Project prefs / knowledge (product) |
| Multi-agent | Teams / channels | `rlm.spawn` children; one `list_agents` roster | Many providers, thread-per-branch | Fork onto other models | Cheap then strong model cascade | Side-by-side providers / fleet |
| UI / control | Product UX + bots | Agents View / ACP | Web, desktop, mobile | CLI (`orca record` …) | GitHub Action + console | Native macOS (usage menu, panes) |
| Contrib posture | PRs + ClawSweeper | Discussions first; vouched + ENG/RES Linear | Active issue triage (`accepted`) | Fail-loud flags/scrub; vision + agent-structure capture | Fail-closed merge gates | Active; packaged-daemon smoke + usage-cache discipline landing |

Update this table when a contribution proves a cell wrong.