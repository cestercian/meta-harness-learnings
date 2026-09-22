# NVIDIA NeMo Agent Toolkit (NAT)

- Repo: https://github.com/NVIDIA/NeMo-Agent-Toolkit
- Role: open-source library for connecting and optimizing teams of AI agents (workflows, tools, LLM/embedder/memory adapters, retries).
- Contrib: Apache-2.0; follow `docs/source/resources/contributing/`. Prefer clear unit repros. Related: NeMo-Agent-Toolkit-UI, NeMo-Relay. Commits need `Signed-off-by`.

## Notes for contributors

- Retry / streaming seams matter: `patch_with_retry` async/sync generator path can swallow provider errors when args are one-shot iterators (see #2223 / open fix #2226). Empty completion after a prior failure must re-raise `last_exception` (prefer `try`/`else` so the re-raise is not caught by the same `except`).
- Reasoning models may wrap a whole ReAct turn in `<think>`; recover Action/Final Answer from inner think text only when it looks like ReAct, and skip replaying empty scratchpad entries (#1611 / merged #2227).
- Adapter flags like `do_auto_retry=False` must be respected end-to-end (#2212 / #2215).
- Prefer fail-loud over empty/silent streams when retries exhaust.

## Related NVIDIA surfaces (secondary unless asked)

- [OpenShell](https://github.com/NVIDIA/OpenShell) — sandboxed agent runtime
- [NemoClaw](https://github.com/NVIDIA/NemoClaw) — run agents inside OpenShell with managed inference
