# 2026-09-18 — NAT / OpenCode / Orca-CR fix patterns from the overnight batch

Overnight contribution wave after the 2026-09-17 NAT+OpenCode map note. Patterns worth carrying, not a PR laundry list.

## NeMo Agent Toolkit — empty retries and think-wrapped ReAct

### Empty stream after a failed retry is still failure (`#2226` open)

`patch_with_retry` generator wrappers treated “attempt finished with zero yields” as success. When a streaming provider error (e.g. 429) is retried with a one-shot async iterator (LangChain-style), the retry consumes an exhausted iterator, yields nothing, and used to return without re-raising `last_exception`. Callers saw a clean empty stream and lost the original error.

Fix posture: re-raise `last_exception` when an attempt completes with no chunks **after a prior failure**, via `try`/`else` so the re-raise is not swallowed by the same `except` and does not burn remaining attempts. A genuinely empty first attempt (no prior failure) still completes empty.

Closes / linked: #2223.

### Reasoning models wrap the whole ReAct turn (`#2227` open)

Some reasoning models put the entire ReAct turn inside `<think>...</think>` with nothing after the close tag. After `remove_r1_think_tags()`, content is empty → parse retry → later cycles can 400 when empty thoughts/tool logs are replayed.

Recover inner `<think>` text **only when** it looks like ReAct (`Action:` / `Final Answer:`). Do not promote plain think text or provider `reasoning_content`. Keep empty-message retry guards; skip replaying empty thought/tool scratchpad entries.

Linked: #1611.

## OpenCode — register early, fence mutations, keep UI/model state honest

Five open fixes on `anomalyco/opencode` share one theme: fail at the boundary the user (or next session) can still correct, not after the catalog or transcript has drifted.

| PR | Seam | Pattern |
| --- | --- | --- |
| `#49573` | Tool registration | Validate opaque `Tool` values with `Tool.validate` **before** mutating the registry; bad batch → `RegistrationError`, previous catalog unchanged. Duplicate plugin copies blow up at register time, not mid-Location materialization. |
| `#49628` | MCP ↔ ToolRegistry | HTTP `add`/`connect`/`disconnect` must **await** a semaphore-serialized reconcile fence so the handler does not return on a stale catalog. Event-driven reconcile stays async (avoids startup deadlock). |
| `#49631` | Attachment → model | TUI showed a filename; model only got bytes + `[Image N]`. Emit a text label like compaction already does (`[Attached image/jpeg: /path]`) before the media part. |
| `#49636` | Interrupt → undo | Admitted-but-unpromoted user input is the TUI undo boundary after interrupt; revert planner must fall back to `session_input.admitted_seq` and delete that input so it cannot promote later. |
| `#49637` | TUI background hint | Hint/`session.background` must treat `state.input.background` (already backgrounded) like `state.metadata.background`, or the “press ctrl+b” prompt lies when every child started backgrounded. |

Issues: #35963, #39902, #41454, #39736, #36940.

## Orca-Code-Review — one authoritative run for reactions and clean verdicts (`#54` open)

Reaction settle and “✅ No findings” publishing used per-site local guesses (hard-coded `github-actions[bot]`, head check only on the thumb step). Superseded runs could publish clean verdicts; custom App installs mishandled 👀.

One planner (`scripts/authority.mjs`) for both call sites:

- **Identity:** prefer the 👀 create-response login, then `/user`, then `{app_slug}[bot]`, with `github-actions[bot]` last.
- **Authority:** publish/settle only when the run still describes the current head and no newer sibling workflow run exists (best-effort if listing is forbidden).
- **👍:** still placed and never withdrawn; stale runs simply do not get `addThumb`.

Linked: #39 (and #40 for thumb permanence).

## Contrib gotcha — pydantic-ai assignment bot

`pydantic/pydantic-ai#8460` (catch `httpx2.InvalidURL` → `ModelRetry` in `WebFetchLocalTool`) was auto-closed: the issue was not assigned to the PR author. Wait for assignment before opening PRs on that repo; comment intent on the issue first.

## Carry-forward

- Retry wrappers: empty completion after a prior failure is failure — re-raise the original exception outside the retry `except`.
- Strip-think / recover-structure only when the stripped content still looks like the agent protocol you need.
- Mutating registries (tools, MCP): validate before mutate; await an explicit reconcile fence on request paths.
- Session / UI surfaces that show a path or undo boundary must use the same identity the model or revert planner uses.
- Multi-site GitHub Action side effects (reactions, clean verdicts) need one authoritative-run planner, not duplicated local guesses.
- On repos with assignment bots, claim before coding.

## Links

- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2226
- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2227
- https://github.com/anomalyco/opencode/pull/49573
- https://github.com/anomalyco/opencode/pull/49628
- https://github.com/anomalyco/opencode/pull/49631
- https://github.com/anomalyco/opencode/pull/49636
- https://github.com/anomalyco/opencode/pull/49637
- https://github.com/Continuum-AI-Corp/Orca-Code-Review/pull/54
- https://github.com/pydantic/pydantic-ai/pull/8460 (closed — unassigned)
