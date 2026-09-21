# NemoClaw (NVIDIA)

- Repo: https://github.com/NVIDIA/NemoClaw
- Role: run agents inside OpenShell with managed inference (local Ollama/vLLM and cloud routes).
- Related: [OpenShell](https://github.com/NVIDIA/OpenShell) (secondary unless asked); pairs with NeMo Agent Toolkit when working NVIDIA surfaces.
- Contrib: **hard cap of 5 open PRs** — surplus opens are auto-closed by `github-actions`. Land or close before opening the next. Prefer scoped fail-loud fixes with live CLI/policy tests.

## Notes for contributors

- Generated MCP-add policies must allow the credential-resolution curl probe or CONNECT 403s self-deny registration (#12104).
- Backups: do not swallow permission-denied dirs then claim a full archive (#12108).
- Local Ollama/vLLM: default progressive tool disclosure to **direct** — nested tool_call/Tool Search breaks those models (#12110).
- Policy remove verification: strip `_provider_*` keys from OpenShell `--base` readback before compare (#12111).
- OpenClaw `devices approve` local fallback must `exit(0)` or NemoClaw `connect` hangs (#12109).