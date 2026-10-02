# 2026-10-02 — T3 verification closes, OrcaRouter timeout≠cooldown, Sotto DCO

Activity after the 2026-10-01 OpenClaw/NemoClaw note (commit `2eced899`). Real posture and architecture signals from T3 closures and a new OrcaRouter-Lite timeout fix; most other babysit traffic was quiet or mechanical.

## T3 — Macroscope approve ≠ merge; UI evidence is a hard close (`#12890`, `#12906` closed)

On 2026-10-01 Julius' automation closed two open UI fixes that Macroscope had already marked approvable:

| PR | Topic | Close reason (verification rule) |
| --- | --- | --- |
| `#12890` | Identical provider-update glyphs must share one popover | Click checks unchecked; no before/after captures or recording. Static markup / callback tests do not prove popover + copy interaction. |
| `#12906` | File↔dir path collision → flat DiffFileTree fallback | No before/after evidence; client-rendering check unchecked. Need colliding diff opening with both paths selecting correctly in PR Code tab and Diff panel. |

Both cite [CONTRIBUTING.md#verification](https://github.com/pingdotgg/t3code/blob/main/CONTRIBUTING.md#verification): match evidence to the change; UI needs clear before/after screenshots (and a short recording when motion/timing matters); attach evidence on the PR, do not commit PR-only media; missing evidence can close before deeper review. Closure notes tell how to reopen (show the interaction, request reconsideration).

**Contrib posture:** treat Macroscope "Approved" as an automation signal, not a merge green light. For any T3 web/desktop UI surface, ship captures with the PR body (or linked artifacts) before expecting human review to stick. Backend-only fixes still need focused evidence, but not unrelated UI screenshots.

Architecture notes from 09-22 (shared `ProviderUpdateAvailableControl`; flat list on prefix collision) remain valid; the PRs died on proof, not on design.

## OrcaRouter-Lite — request timeout is not cooldown (`#166` open)

`OrcaLiteLLMClient` hardcoded LiteLLM `Router(timeout=30.0)`. `router_cooldown_seconds` only controls how long a deployment stays cooled down *after* a failure. Slow reasoning models that legitimately take 45–60s were hitting `upstream_timeout` and getting cooled while healthy.

**Fix posture:** expose `ROUTER_TIMEOUT_SECONDS` (default 30, behavior-preserving) through settings → `router_cache` → adapter. Document next to cooldown so operators do not assume one knob covers both.

| Knob | Meaning |
| --- | --- |
| Router `timeout` | How long a single upstream call may run before LiteLLM treats it as timeout |
| `router_cooldown_seconds` | How long a deployment stays out of rotation after a failure |

Same class of bug as treating keepalive chunks as model progress (OpenClaw `#162018`): two timers with similar names, different jobs.

## Sotto — DCO Signed-off-by on every commit (`#473`, `#474` open)

First PRs to getsotto/sotto triggered the welcome bot: every commit needs `Signed-off-by` (`git commit -s` / `git rebase --signoff main`), plus `cargo fmt --all --check` and `cargo clippy --workspace --all-targets -- -D warnings`. Both open fixes (`#473` history popup actions while scrolling; `#474` New secret name collision) already carry the trailer. Treat DCO as a hard merge gate on this surface, same class as NVIDIA Verified / ClawSweeper prove-on-main.

## Status only (no new architecture lesson)

- rocketmq-rust `#11073` merged (rename `MappedFileDestroyOutcome` → `MappedFileRemovalStatus`); `#11072` docs open.
- termlens `#550` / `#551` merged (Result-returning test; `mask_matching` doctest).
- vortix `#377` merged after a docs precision fix (no install channel ships completions yet — do not imply Cargo/npm do).
- markview `#36` closed by maintainer in favor of their own commit (`243f072`) — docs PR superseded, not a harness lesson.
- cdk `#2638`, OrcaRouter `#166`, sotto `#473`/`#474`, hivegate `#76` (approved), nginx `#10912` (review fixes landed; deterministic warning order + non-destructive `UpdateStatus`) — babysit / quiet.
- OpenClaw `#162018` / `#162033`, NemoClaw `#12539`, Hermes `#123108`/`#123116`, Omnigent `#8144`/`#8312`, foreman `#24`/`#25`, JevHarness `#2`, T3 `#13295`/`#12889`/`#12878`/`#12863`/`#12633` — no new durable review signal since 10-01. Prime still vouch-gated.

## Carry-forward

- T3 UI: attach before/after (and recordings when needed) on open; Macroscope alone will not protect against verification closure.
- Router / agent timeouts: name and configure **request timeout** separately from **cooldown / idle / progress** deadlines.
- New contrib surfaces: read the welcome / CONTRIBUTING gate (DCO, clippy -D, prove-on-platform) before the second push.
- Prior 10-01 idle≠keepalive, Windows `\\?\` normalize, and sandbox-scoped restore carry-forwards still stand.

## Links

- https://github.com/pingdotgg/t3code/pull/12890
- https://github.com/pingdotgg/t3code/pull/12906
- https://github.com/pingdotgg/t3code/blob/main/CONTRIBUTING.md#verification
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/166
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/issues/110
- https://github.com/getsotto/sotto/pull/473
- https://github.com/getsotto/sotto/pull/474