# 2026-09-14 — OrcaReplay: vision agents, scrub false all-clear, Windows red baseline

Upstream Continuum merges since the 2026-09-11 fail-loud / capture note. Not our PRs — architecture posture to carry when contributing.

## Vision capture: each half correct, the seam broken (`#70`)

A browser agent (browser-use) puts a screenshot (`data:` URI / base64 PNG) in every model request. Two independently defensible components made that impossible to record or replay:

1. **Redactor entropy sweep** treats long random-looking runs as leaked keys. Agent-encoded images are base64 by construction — same class as the proxy’s own base64 path that `#scan` already skipped (comment: shredded a 14 KB Connect/protobuf into 226 placeholders). Measured Wikipedia run: **47,314** `<secret:high_entropy:…>` placeholders (every one a PNG fragment) → **6** after the fix; screenshots intact.
2. **Matcher** compares what it was given. After shredding, recorded bytes ≠ sent bytes, so replay cannot match *by construction*.

Fixes that land together:

- Do not entropy-shred agent-encoded image payloads the way secrets are shredded.
- Match ladder **folds image pixels below rung 1** (presence / fold named in divergence; pixels not compared). Accompanying text (DOM tree, URL, agent memory) stays fully compared — blinding pixels does not force false matches.
- Cross-seam integration `vision-repaint`: record noise PNG → assert intact + no entropy placeholder → replay with a *different* PNG → assert match survives and reports the fold. Remove either half → red for the right reason.

**Ceiling (honest):** even after orca is faithful, live-world agents still diverge (browser-use renumbers elements each load). Orca should report that, not patch the agent.

## Scrub: silent under-count is worse than no scrubber (`#78`)

`walk` failed in both directions:

- Unreadable prefix directory → returned `[]` → blob pass scrubbed *fewer* blobs and reported the smaller count as complete (exit 0, no warning). Repro: deny `blobs/f7` → `would_remove` 6→5 with a green dry-run. SECURITY.md names this: a scrubber that hands a false all-clear is worse than none.
- Unreadable / raced `stat` (dangling symlink, TOCTOU delete) → threw and **abandoned** an otherwise complete scrub.

Same fail-loud family as TLS `--model` and flag-value fallbacks: unsupported or partial I/O must not look like success.

## Windows red baseline hid a real bug (`#69`)

Local Windows sat at **23** failures while Linux CI was green. Cost was verification poisoning: pre-existing red read as newly introduced and vice versa. One real bug among them: bare Windows absolute paths are not ESM specifiers (`ERR_UNSUPPORTED_ESM_URL_SCHEME` — need `file://`). Noise baseline made the real failure message invisible behind count assertions.

## Carry-forward

- Capture / redaction / match must be designed **across the seam**, especially for multimodal payloads.
- Security tooling (scrub) must fail loud on unreadability — never shrink the reported set.
- Platform-local red baselines are a contrib hazard; prefer fixing or quarantining them before trusting local green/red.

## Links

- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/70
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/78
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/69
- Related: `learnings/2026-09-11-orca-fail-loud-and-capture.md`, `learnings/2026-09-15-orca-agent-structure-and-empty-capture.md` (#65 merged)