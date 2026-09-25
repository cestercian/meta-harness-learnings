# 2026-09-25 — OrcaRouter-Lite merge wave + wire/placeholder review lessons

Activity after the 2026-09-24 morning note (commit `c26eb62`). Six OrcaRouter-Lite fixes merged the same day; maintainer end-to-end review on the still-open set produced the patterns worth carrying.

## Merged — request fidelity and auth/cache honesty landed

| PR | Seam | Pattern confirmed |
| --- | --- | --- |
| `#147` | `parallel_tool_calls` | Undeclared on the Pydantic chat schema → silent drop; `False` must still forward via `model_dump(exclude_none=True)`. |
| `#149` | `logit_bias` | Same silent-drop class; include the map in the prompt-cache key so different biases cannot share an entry. |
| `#150` | Auth candidates | Last-wins across headers hid useful errors. Rank specificity (`revoked` > `invalid` > `format`); keep first on a tie. |
| `#153` | Hosted fallback | When local BYOK 429s and hosted serves, set `x-orca-fallback: true` (and log `fallback_level=1`). Hosted-only stays quiet. |
| `#155` | Prompt cache | Key space bumped to `v: 2`. `top_p < 1` needs a `seed` to count as deterministic; omitted temperature still does not imply 0. |
| `#146` | Legacy crypto | Cover the real `0x01` nonce false-positive path (v1 framing raises `InvalidTag`, public decrypt still returns plaintext). |

Carry-forward unchanged from 2026-09-21: declare OpenAI-compatible fields on the router schema, or clients look healthy while the upstream never sees the flag.

## Open — synthesize usage only when the wire actually lacks it (`#154`)

The "emit a dedicated usage SSE chunk before `[DONE]`" fix regresses token accounting on current `main`. LiteLLM already emits a usage frame when the router injects `stream_options.include_usage=True` (the default). That frame carries `choices=[{"index":0,"delta":{}}]` — non-empty — so a dedupe that required empty `choices` always synthesized a second frame. Clients that sum `usage` across frames (several SDKs, LangChain) double-count.

Lessons:

1. **Match the upstream frame shape.** Treat any trailing frame with `usage` as sufficient; do not require empty `choices`.
2. **Measure the wire before synthesizing.** If every stream already gets a usage frame (or `include_usage: false` yields none to collect), the original "clients that stop at `finish_reason`" bug may be unreachable on this codebase. Closing can be cleaner than landing a synthesis that CI encodes as the expected duplicate.
3. **Tests that assert the synthesized frame** will stay green while the wire is wrong. Revisit assertions with the condition, not one-line patches alone.

## Open — dashboard placeholders are not keys (`#145`)

BYOK write-time prefix warnings now reach the client (obvious Groq/`gsk_` vs OpenAI/`sk-` miss). Remaining gotcha from maintainer verify:

- The dashboard sends `[REDACTED]` when the operator saves without retyping. Format checks that do not early-return fire a bogus "prefix `[REDA` doesn't match" warning on that ordinary path.
- Worse, and pre-existing on `main`: `PUT` with `[REDACTED]` **stores the placeholder as the credential**, overwriting the real key. Treat the placeholder as "no change" (or reject it) before format checks and before persist.

Same family as Omnigent probe credential symlink refresh: control-plane write paths must distinguish "operator left the masked field alone" from "operator typed a new secret."

## Open — allowlist write path: resolve threads + answer TOCTOU (`#151`)

Write path for `model_allowlist` (POST create, PUT update, catalog 422, `null` = unrestricted, `[]` = deny-all) verifies end-to-end. Escalation vectors (restricted key minting unrestricted / wider sibling / widening self) are blocked.

Two process lessons:

1. **`required_review_thread_resolution: true`** — a later bot `✅ No findings` does not auto-close older P1 threads. Resolve the threads your pushes covered or the PR stays unmergeable.
2. **Auth-time snapshot vs commit-time check** — the clear/narrow decision reads `kc.model_allowlist` captured at authentication. Either CAS / re-read at write, or put on the record why the TOCTOU window is acceptable (single-row write, bounded worst case). Silent resolve is not enough when the maintainer asked for reasoning.

Also still open: `#148` (`max_completion_tokens` forward; rebase after sibling `cache_key` change), `#156` (LangChain `json_schema` shape normalize; rebase after `#147` tests).

## Carry-forward

- Router schemas: declare every OpenAI-compatible field clients send; put sampling / bias / cap fields in the cache key.
- Streaming: measure whether usage already arrives before synthesizing; dedupe against real upstream frame shapes.
- Credential UIs: `[REDACTED]` / masked placeholders mean "no change", never a key to validate or store.
- Allowlist / key governance: restricted principals cannot widen; catalog-validate; resolve review threads under rulesets that require it; answer TOCTOU on the record.
- Auth errors: prefer the most specific message across credential candidates.

## Links

- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/147
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/149
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/150
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/153
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/155
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/146
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/154
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/145
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/151
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/148
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/156
