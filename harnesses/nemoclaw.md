# NemoClaw (NVIDIA)

- Repo: https://github.com/NVIDIA/NemoClaw
- Role: run agents inside OpenShell with managed inference (local Ollama/vLLM and cloud routes).
- Related: [OpenShell](https://github.com/NVIDIA/OpenShell) (secondary unless asked); pairs with NeMo Agent Toolkit when working NVIDIA surfaces.
- Contrib: **hard cap of 5 open PRs** — surplus opens are auto-closed by `github-actions`. Land or close before opening the next. Prefer scoped fail-loud fixes with live CLI/policy tests.

## Notes for contributors

- MCP credential probe: use the adapter runtime (Node fetch / urllib), not curl on the generated binary allowlist; `#12104` superseded by maintainer salvage `#12175` merged (closes #12065).
- Backups: do not swallow permission-denied dirs then claim a full archive (#12108).
- Local Ollama/vLLM: default progressive tool disclosure to **direct** — nested tool_call/Tool Search breaks those models (#12110).
- Policy remove verification: strip `_provider_*` keys from OpenShell `--base` readback before compare (#12111).
- OpenClaw `devices approve` local fallback: drain stdout/stderr then `exit(0)` after Approved so leftover gateway handles do not hang `nemoclaw connect`. `#12109` superseded by maintainer salvage `#12178` merged (closes #12064).