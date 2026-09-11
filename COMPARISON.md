# Comparison snapshot (2026-09)

| Dimension | OpenClaw | Prime Agent | T3 Code | OrcaReplay | OrcaCode Review |
| --- | --- | --- | --- | --- | --- |
| Primary job | Always-on multi-channel assistant | Long-horizon coding/research agent | Control surface over agent CLIs | Capture & time-travel agent runs | Automated PR review |
| Session model | Durable product sessions | Daemon + worker + IPython RLM | Server owns sessions; clients over WS | Recorded trajectory / fork points | Per-PR / per-push review runs |
| Tools | Channel + product tools | Programmatic tools via REPL | Delegates to provider CLIs | Intercepts / records tool+model I/O (+ origin) | Review engine + gateway |
| Memory / skills | Product memory; ClawSweeper automation | Continual Harness (ρ,G,K,M) + `/refine` | Provider-native + orchestration | Replay artifacts | Rubric / policy in router |
| Multi-agent | Teams / channels | `rlm.spawn` children; one `list_agents` roster | Many providers, thread-per-branch | Fork onto other models | Cheap then strong model cascade |
| UI / control | Product UX + bots | Agents View / ACP | Web, desktop, mobile | CLI (`orca record` …) | GitHub Action + console |
| Contrib posture | PRs + ClawSweeper | Discussions first; vouched + ENG/RES Linear | Active issue triage (`accepted`) | Fail-loud flags; smaller issue surface | Fail-closed merge gates |

Update this table when a contribution proves a cell wrong.