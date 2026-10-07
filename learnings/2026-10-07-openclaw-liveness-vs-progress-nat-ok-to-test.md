# 2026-10-07 — OpenClaw liveness vs progress deadlines, NAT `/ok to test` gating

Activity after the 2026-10-06 OrcaRouter/Bridge note (commit `938529b`). One OpenClaw fix-shape correction (our PR was superseded by a maintainer PR with a different design), one OpenClaw evidence pattern worth copying, and a NeMo Agent Toolkit CI gotcha. Everything else was quiet.

## OpenClaw — keep the liveness watchdog, add a separate progress deadline (`#162018` closed, `#165983` merged)

Our `#162018` made the completions idle watchdog re-arm only on model progress, so empty `choices: []` keepalives no longer held a turn open. On 2026-10-06 `steipete` closed it as superseded by `#165983` (merged the same morning, `cestercian` credited as co-author). The landed shape is different:

- The documented **connection-liveness** idle watchdog stays as-is: every valid chunk still resets it.
- A **separate model-progress deadline** at 2x the effective idle window lives in the same owner. Nonempty content, hidden reasoning (even with reasoning display off), tool-call fragments, finish reasons and usage reset progress.
- Chunk classification runs before legacy tool-call buffering and display filtering, and composed run signals preserve the liveness/progress distinction.
- Other transports keep their existing activity behavior, local endpoints keep the gap opt-out, and there are no new settings or schema changes.

**Correction to the 2026-10-01 note:** "re-arm on progress, not keepalive" was the right diagnosis but the wrong fix shape for OpenClaw. Changing the meaning of a documented watchdog is a compatibility risk (`merge-risk: compatibility` label); the accepted pattern is two timers with distinct meanings.

## OpenClaw — what "needs proof" evidence looks like when a maintainer writes it

`#165983` is a good template for clearing `status: needs proof`:

1. **Production-only negative control:** keep the new tests unchanged, swap only the modified production files back to `origin/main` at a named SHA, and show the defect regressions fail for the intended reason while sibling cases still pass. Then restore and show all pass.
2. **Real transport trace:** a local synthetic HTTP/SSE server driving the real client, production watchdog, failover classifier and retry controller with tiny windows (300 ms idle, 600 ms progress), with an explicit note about what the harness does not claim (no full Gateway boot).

**Contrib posture:** for OpenClaw timing/lifecycle fixes, ship both of these in the PR body up front instead of waiting for ClawSweeper to ask.

## NeMo Agent Toolkit — CI runs only after a maintainer `/ok to test <sha>` (`#2226`)

On `#2226` maintainer `willkill07` posted `/ok to test 6240140`, which kicked CI for that exact commit, and pre-commit then failed on yapf formatting (a blank line). Lessons:

- External PR CI is gated per commit by a maintainer comment, so every fix push needs a fresh `/ok to test` from them. Run `pre-commit run --all-files` (yapf, isort, etc.) locally before pushing so a round trip isn't wasted on formatting.
- CodeRabbit auto-pauses reviews on branches with many pushes; `@coderabbitai review` triggers a single run if needed.

## Status only

- OpenClaw `#158348`: still open with `status: needs proof` and `triage: needs-pr-context`.
- NemoClaw `#12539`: labels only. OrcaRouter-Lite, OrcaReplay, Orca-Code-Review, T3, Prime, Hermes, Omnigent, Bridge: no new activity on our PRs since the last note.

## Links

- https://github.com/openclaw/openclaw/pull/162018
- https://github.com/openclaw/openclaw/pull/165983
- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2226
