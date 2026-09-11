# Prime Agent

- **Repo:** https://github.com/PrimeIntellect-ai/prime-agent
- **Role:** Self-improving coding/research harness (RLM + Continual Harness).
- **Key ideas:** Persistent IPython as the membrane; harness state `H=(ρ,G,K,M)` refined via `/refine`; daemon/worker; recursive subagents via **`await rlm.spawn(..., name=...)`** (not `rlm.run` / callable `rlm`); single roster `agent_observe.list_agents()`.
- **Contribution posture (hard):** Public intake is **GitHub Discussions**. Issues are maintainer work queue. Unsolicited / unvouched PRs are auto-closed (`contribution-gate.yml` + vouch). Earn invite via Discussions, investigation, docs, testing.
- **When vouched:** Linear ticket gate requires `ENG-###` or `RES-###` (or `No-Ticket: …`) in title/body/branch — ENG = V1 board, RES = Long-Horizon.
- **Do not:** Drive-by agent PRs. Start in Discussions; wait for maintainer invite before substantial code.