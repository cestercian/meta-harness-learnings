# NVIDIA NeMo Agent Toolkit (NAT)

- Repo: https://github.com/NVIDIA/NeMo-Agent-Toolkit
- Role: open-source library for connecting and optimizing teams of AI agents (workflows, tools, LLM/embedder/memory adapters, retries).
- Contrib: Apache-2.0; follow `docs/source/resources/contributing/`. Prefer clear unit repros. Related: NeMo-Agent-Toolkit-UI, NeMo-Relay.

## Notes for contributors

- Retry / streaming seams matter: `patch_with_retry` async-generator path can swallow provider errors when args are one-shot iterators (see #2223).
- Adapter flags like `do_auto_retry=False` must be respected end-to-end (#2212 / #2215).
- Prefer fail-loud over empty/silent streams when retries exhaust.

## Related NVIDIA surfaces (secondary unless asked)

- [OpenShell](https://github.com/NVIDIA/OpenShell) — sandboxed agent runtime
- [NemoClaw](https://github.com/NVIDIA/NemoClaw) — run agents inside OpenShell with managed inference
