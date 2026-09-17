# 2026-09-17 — OrcaReplay: gatewayable traces at write time; pull verifies; LangGraph nodes

Upstream Continuum since the 2026-09-16 MCP-check / gateway-fork note. Not our PRs — posture to carry.

## Locally pretty is not gatewayable (`#90` merged)

Spec §2.3 wants exactly one `run.start` and one `run.end`. Nothing enforced it. Three commands each wrote a different seq-0 event that `orca show`, `orca replay`, and the viewer happily render — then `orca push` rejects the archive:

| command | seq 0 | pushable |
| --- | --- | --- |
| `record --mcp-config` | a `note` | no (earlier fix in `#85`) |
| `attach` | `model.request`, and no `run.end` | no |
| `replay --from N` / `compare` | `fork` | no |

The cost of finding out late is the whole recording: the user learns at push, after the agent session is over, that there is nothing to push. Enforce the bracket in the writer / command path (or refuse early), not only at the gateway.

## Pull must verify what it wrote (`#91` merged)

Spec §6: readers SHOULD verify and MUST report, not repair, a mismatch. `verifyIntegrity` existed and `replay` called it; `pull` wrote the run and said `pull.done`. Zip CRC catches a flipped bit. It cannot catch an internally consistent archive whose events are not the ones the manifest attests to — the shape a far-side storage bug takes. Verify against the manifest root on the staged copy **before** the swap so a failed verification cannot replace a good local run.

## LangGraph nodes the proxy cannot see (`#92` merged)

Second framework adapter (`orcareplay-langgraph`). A LangGraph run's node names and graph shape are not on the wire. Validators, routers, and writers that call no model are invisible to a proxy, so two different graphs can look identical. Record `graph.node.start` / `graph.node.end` (including no-model nodes). Same family as openai-agents agent-structure (`#65`): off-wire structure needs an adapter, and schema / viewer must know the events.

## Watchlist (open, not landed)

Continuum also opened a Windows / test-honesty cluster (`#93`–`#98`): named failure events instead of one catch-all handshake failure, sorting permanent-red Windows baselines so the eleventh failure is visible, `chmod 0600`/`0700` noop on Windows, tests that must not read or call the developer's real gateway, and a doctor false-positive on the Windows shell shim. Treat as follow-on fail-loud / CI hygiene — do not claim merged yet.

## Carry-forward

- Enforce gateway / spec invariants at write time (or refuse early), not only at `push`.
- Every read path that materializes a run (`pull` included) should verify integrity before declaring success or swapping.
- Framework adapters that record off-wire structure (LangGraph nodes, openai-agents handoffs) are first-class capture layers — keep them in lockstep with schema, viewer, and checks.

## Links

- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/90
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/91
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/92
- Related: `learnings/2026-09-15-orca-agent-structure-and-empty-capture.md`, `learnings/2026-09-16-orca-mcp-check-and-gateway-fork.md`