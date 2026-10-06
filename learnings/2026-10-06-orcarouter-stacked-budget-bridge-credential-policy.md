# 2026-10-06 — OrcaRouter stacked budget PRs, Bridge credential policy and orchestrator notices

Activity after the 2026-10-05 Omnigent/OpenClaw/Bridge note (commit `ca8b238`). One contribution-shape lesson from OrcaRouter-Lite, a review-trigger posture fact, and three Bridge architecture lessons that shipped in the 0.9.0 release. Everything else was labels and quiet.

## OrcaRouter-Lite — big features land as a stack, library first (`#160` merged)

The budget-enforcement feature (`#91`) was split into a four-PR stack for reviewability, and `[1/4]` merged on 2026-10-06:

| PR | Lands |
| --- | --- |
| `#160` (merged) | `spent_microcents` column, startup migration, `packages.auth.spend` library + unit tests; no request-path change |
| `#161` | Wires `is_exhausted` / `charge_budget` into `execute_chat` (the actual enforcement) |
| `#162` | Parks settlements that outlive their retries, folds them at pre-check |
| `#163` | Fair pricing for adapter faults and client hangs |

OrcaCode Review flagged the library as a P1 "dead code" finding (zero callers in `app/`). The author answered on the PR body that this is the intended shape: a landable-but-unused module gets the schema and atomic-charge semantics reviewed on their own before they touch anyone's requests, and the wiring is already up in the next PR. The finding cleared to zero on a later push and the PR merged.

**Contrib posture:** for any OrcaRouter change that alters request-path behavior, splitting into "schema + library, no callers" then "wiring" is accepted, and a review-bot dead-code P1 on the first slice is answered with the stack table, not by inlining the wiring.

## OrcaRouter-Lite — OrcaCode Review runs are maintainer-triggered (`#166`)

On 2026-10-06 maintainer `xizhuomengcontin` commented `@orcacode-review` on `#166` (configurable LiteLLM Router timeout); the bot returned "No findings" a few minutes later. Contributors do not need to re-request the bot after pushes; a maintainer-triggered clean run is the readiness signal, and the merge is still a human step.

## Bridge — deployment topology and credential policy are independent (`#815`, in 0.9.0)

- `ExecutionTopology` (embedded, local-daemon, remote-runner) and `CredentialPolicy` (user-managed, api-key-only, enterprise-managed) are separate axes, so where a harness runs never decides how it is billed.
- Under `api-key-only`, a subscription login is refused before a Codex thread opens or a Claude sidecar spawns, and the subscription token is neither injected nor inherited. Refusal is a stable error code (`credential_policy_violation`, 1006).
- Adapters whose credential source Bridge cannot identify are refused under a restrictive policy rather than waved through.
- An unparseable `BRIDGE_CREDENTIAL_POLICY` fails closed to `api-key-only`, never to the permissive default.
- The new opt-in WebSocket listener on `bridged` reuses the Unix-socket connection layer behind one frame source/sink (same handshake token, frame cap, connection cap, deadline). Browser `Origin` must match an exact allowlist; a non-loopback bind needs an explicit `--allow-remote-bind` because there is no TLS yet.

## Bridge — every orchestrator notice costs a model turn (`#818`, in 0.9.0)

Worker routing notices to an orchestrator each trigger a turn, so Bridge added notice levels (`all`, `actionable`, `results-only`) with a global setting and a per-orchestrator override. Every notice type is classified in one place and delivered through one choke point; identical informational notices coalesce within 30s with a `coalescedCount`; failures, declined scopes, stops and peek replies are never suppressed; and the orchestrator prompt is told filtering exists so it peeks instead of waiting.

**Design lesson for any multi-agent harness:** treat child-to-parent notifications as a token budget, filter at a single classified choke point, and tell the parent model when it is being filtered.

## Bridge — deferred MCP tools are not a per-turn cost (`#775`)

With Claude Code tool search on, an MCP tool's schema is sent only after the model looks it up, so a deferred tool costs roughly its name per turn. The SDK's `getContextUsage().mcpTools[]` still reports every schema's full size with an `isLoaded` flag; Bridge's context lens now sums only loaded tools and shows "L of N tools loaded". Any harness that budgets context from SDK usage reports must honor the loaded flag or it will overstate MCP cost by tens of thousands of tokens.

## Bridge — release cadence after `#638`

Release Please cut 0.7.0 on 2026-10-03 and 0.9.0 on 2026-10-05 (`#781`, `#809`). The serialized protected-main pipeline from the 10-05 note is now producing near-daily stable releases, so conventional squash titles directly shape public changelogs.

## Status only

- Hermes `#133584` (blocked SSH port 22 on update fetch): triaged P2 with `sweeper:risk-compatibility`, no review yet.
- NemoClaw `#12342`: labeled docs/chore; other NemoClaw PRs only got bulk metadata touches.
- OpenClaw `#162018`, `#158348`: quiet. Prime: still vouch-gated, nothing new.

## Links

- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/160
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/166
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/815
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/818
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/775
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/809
