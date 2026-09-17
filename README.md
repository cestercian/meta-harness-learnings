# Meta Harness Learnings

Living notes on **AI coding agent harnesses** — how they run sessions, tools, memory/skills, multi-agent work, and control planes — plus lessons from contributing to them.

Maintained by [cestercian](https://github.com/cestercian) while shipping patches across this ecosystem.

## Why this repo

Harnesses are the runtime membrane around a model: tools, persistence, refinement, UI/control, recovery. Comparing them (and writing down what breaks in the wild) compounds faster than tribal memory in chat logs.

## Harness map

| Harness | Role | Repo | Notes |
| --- | --- | --- | --- |
| **OpenClaw** | Multi-channel agent product (OSS we already contribute to) | [openclaw/openclaw](https://github.com/openclaw/openclaw) | MIT; in-use contribution target |
| **Prime Agent** | Self-improving RLM + Continual Harness | [PrimeIntellect-ai/prime-agent](https://github.com/PrimeIntellect-ai/prime-agent) | Discussion-first; PRs only if vouched |
| **T3 Code** | Control plane for coding agents | [pingdotgg/t3code](https://github.com/pingdotgg/t3code) | Orchestrates Claude/Codex/Cursor/Grok/OpenCode |
| **Bridge** | Native macOS control room for coding agents | [Atharva-Kanherkar/bridge-harness](https://github.com/Atharva-Kanherkar/bridge-harness) | Peer to T3; Apple Silicon–first; active PRs |
| **NeMo Agent Toolkit** | Connect/optimize multi-agent workflows | [NVIDIA/NeMo-Agent-Toolkit](https://github.com/NVIDIA/NeMo-Agent-Toolkit) | NVIDIA; retries/adapters; prioritize with OpenCode |
| **OpenCode** | Open-source coding agent | [anomalyco/opencode](https://github.com/anomalyco/opencode) | Used under T3/Bridge; large bug surface |
| **OrcaReplay** | Record / replay / fork agent runs | [Continuum-AI-Corp/OrcaReplay](https://github.com/Continuum-AI-Corp/OrcaReplay) | Capture harness for debugging agents |
| **OrcaCode Review** | PR review harness + merge gate | [Continuum-AI-Corp/Orca-Code-Review](https://github.com/Continuum-AI-Corp/Orca-Code-Review) | Cheap→strong cascade via OrcaRouter |
| **OrcaRouter-Lite** | Self-hosted LLM router | [Continuum-AI-Corp/OrcaRouter-Lite](https://github.com/Continuum-AI-Corp/OrcaRouter-Lite) | Gateway / BYOK |
| **sandbox-runtime** | OS-level FS/network sandbox for agents | [anthropics/sandbox-runtime](https://github.com/anthropics/sandbox-runtime) | Forked as `cestercian/sandbox-runtime` |

Detail pages live under [`harnesses/`](./harnesses/). Cross-cutting comparison: [`COMPARISON.md`](./COMPARISON.md). Dated notes: [`learnings/`](./learnings/).

## Our current OSS harness

We treat **OpenClaw** as the open-source harness already in orbit (prior upstream PRs; ClawSweeper review culture; Telegram/outbound delivery edge cases). Supporting forks in the same workspace: `sandbox-runtime`, `claude-agent-sdk-python`, `agent-framework`, `agent-skills-kit`.

Contribution *process* harness (how we ship): Cursor cloud agents on forks → parent review → upstream PR → GitHub babysit routines that escalate every human/bot comment and self-delete on merge/close. Rules: commits authored as `cestercian` only; never AI `Co-authored-by`; never open upstream before parent review.

## Contributing to this notes repo

Append dated files under `learnings/YYYY-MM-DD-slug.md`. Prefer facts with links (issue/PR/commit). Keep diffs of understanding, not marketing.

## License

MIT

## Next-tier map (2026-09-17)

See `learnings/2026-09-17-harness-map-expansion.md` for OpenHands, Cline, Goose, Aider, SWE-agent, Codex CLI, browser-use, and framework-adjacent control planes.
