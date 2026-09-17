# Harness map expansion (2026-09-17)

GitHub search hit rate limits mid-run; this pass used direct `repos/{owner}/{name}` lookups plus public 2026 field guides (FutureAGI harness guide, OpenHands OSS agents post, Pinggy CLI agents roundup).

## Already prioritized (keep)

| Repo | Stars (approx) | Role |
| --- | ---: | --- |
| openclaw/openclaw | 390k | Agent OS / multi-surface |
| anomalyco/opencode | 208k | Terminal coding agent |
| pingdotgg/t3code | 23k | Coding agent product |
| PrimeIntellect-ai/prime-agent | 21k | RLM coding agent |
| Continuum Orca* | 0.2–1.2k | Replay / review / router |
| NVIDIA/NeMo-Agent-Toolkit | 2.6k | Agent toolkit |
| NVIDIA OpenShell / NemoClaw | 9–22k | Secure agent runtime (secondary) |

Bridge-harness stays out of scope unless asked.

## Strong next-tier coding agents / harnesses

| Repo | Stars | Why it matters |
| --- | ---: | --- |
| OpenHands/OpenHands | 88k | Autonomous event-stream coding agent platform |
| cline/cline | 69k | IDE + CLI + SDK coding agent |
| aaif-goose/goose | 54k | Extensible MCP-heavy agent (Block lineage) |
| Aider-AI/aider | 49k | Git-native terminal pair programmer |
| openai/codex | 125k | Official Codex CLI coding agent |
| anthropics/claude-code | 146k | Official Claude Code agent (limited OSS surface) |
| SWE-agent/SWE-agent | 20k | ACI / SWE-bench research harness |
| continuedev/continue | 36k | OSS coding agent in editor (Cursor-acquired; still public) |
| RooCodeInc/Roo-Code | 24k | Cline-family IDE agent (product evolving) |
| voideditor/void | 29k | OSS editor with agent hooks |
| vercel-labs/agent-browser | 43k | Browser automation CLI for agents |
| browser-use/browser-use | 115k | Browser-using agents |

## Framework / control-plane adjacent (not full coding harnesses)

| Repo | Stars | Notes |
| --- | ---: | --- |
| langchain-ai/langgraph | 42k | Agent graphs / runtime |
| microsoft/autogen | 61k | Multi-agent framework |
| crewAIInc/crewAI | 59k | Role-playing multi-agent |
| pydantic/pydantic-ai | 20k | Typed Python agents |
| huggingface/smolagents | 29k | Code-thinking agents |
| google/adk-python | 22k | Google ADK |
| camel-ai/camel | 18k | Multi-agent research |
| elizaOS/eliza | 19k | Agentic OS |
| PrimeIntellect-ai/prime-rl | 2k | Agentic RL training |
| Continuum-AI-Corp/OrcaRouter-Lite | 1.2k | LLM router control plane |
| Significant-Gravitas/AutoGPT | 187k | Broad agent platform (noisy) |

## Contribution posture

Prefer next coding targets with open `bug` issues and fork+CloudAgent access: **OpenHands**, **Cline**, **Goose (aaif-goose)**, **Aider**, **SWE-agent**, **Codex CLI**, then **browser-use / agent-browser** when the bug is harness-adjacent. Keep NVIDIA OpenShell/NemoClaw secondary unless asked.
