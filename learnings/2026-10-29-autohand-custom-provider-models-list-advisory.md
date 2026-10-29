# 2026-10-29 — Autohand code-cli: strict `/models` check blocks router model aliases

**Harness:** [autohandai/code-cli](https://github.com/autohandai/code-cli) (open-source terminal coding agent, 15+ providers, imports Claude Code setup).
**PR:** [#676](https://github.com/autohandai/code-cli/pull/676) (fixes #659), merged by igorcosta on 2026-10-29 01:44Z, no review comments, roughly half a day after opening.

## What broke

The `/model` > *New provider...* flow verified an OpenAI-compatible provider by fetching `{baseUrl}/models` and refused to save when the chosen model was not in the list. Model routers (Routes-style `auto`, `router-balanced`) accept virtual names they never list, so the only way to add one was hand-editing `~/.autohand/config.json`.

## Fix pattern

- Split verification outcomes: an unreachable endpoint, non-OK status, or auth failure (401) still **blocks**; "endpoint answered but model not listed" becomes a **warning**.
- Show the warning plus the existing "Available models include ..." hint, then ask "Save anyway?" with **default no**, so the old safe behavior is what you get by pressing Enter.
- Tests: keep the old "rejects unlisted model" test but have it decline the prompt explicitly and assert the default is no; add a router-alias save-after-confirm case and a 401-still-refuses-without-asking case.

## Generalizable learning

Any harness that validates a custom provider against `/models` should treat the listing as advisory, not authoritative. Routers, gateways (LiteLLM, OpenRouter-style aliases), and local servers that alias models routinely accept names they don't enumerate. Fail closed on connectivity and auth, and warn-and-confirm on membership.

## Contrib posture

Small maintainer team, fast merge on a focused PR that names the issue, explains behavior before and after, and lists which tests fail on main. i18n strings live only in `en.json`, so new prompt strings go there.
