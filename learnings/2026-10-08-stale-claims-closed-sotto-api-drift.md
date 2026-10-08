# 2026-10-08 — Stale claimed issues closed upstream, Sotto API drift after rebase

Activity after the 2026-10-07 OpenClaw/NAT note (commit `fe56a3f`). Two issues we had claimed weeks ago were closed by maintainers without a PR, and a Sotto review surfaced a compile break that a clean git merge would have hidden. Everything else was quiet.

## Re-verify a claim on the latest release before working it

Two claims from 2026-09-18/19 went stale and were closed upstream this week:

- **NemoClaw `#12059`** (`NEMOCLAW_CORPORATE_CA_BUNDLE` made sandbox creation fail with an unrelated managed-image-catalog error). Claimed 2026-09-19, never shipped. On 2026-10-07 a contributor re-ran the issue's own A/B/C control sequence on v0.0.131 and the symptom no longer reproduced; `rsliter` closed it as completed.
- **OpenCode `#36766`** (truncated OpenAI tool-call arguments). Claimed 2026-09-18. On 2026-10-08 `rekram1-node` closed it as not planned: it was specific to an early OpenCode v2 pre-release and no longer relevant.

**Contrib posture:**

- Before starting (or resuming) work on a claim older than about a week, reproduce on the newest release or `main` first. Fast-moving harnesses fix things incidentally.
- For OpenCode, check the issue's age and which version line it was filed against. Issues from the early v2 period (mid-2026) may describe code that no longer exists.
- Drop or release a claim if nothing ships within a couple of weeks, so the issue doesn't look owned.

## NemoClaw — corporate CA bundle file mode

From the `#12059` re-check: on v0.0.131 a group-writable CA bundle (`0664`) fails onboarding at step 6/8 with a CA-named permission error. `0644` and `0600` work, and the CA is genuinely trusted inside the sandbox (`/usr/local/share/ca-certificates/nemoclaw-corporate-ca-01.crt`, linked into `/etc/ssl/certs`). If a CA-related onboarding repro comes up, check file mode before anything else.

The re-check itself is a good evidence template: controls without the variable run before and after the test runs, each sandbox gets its own name and `--control-ui-port`, and runs go sequentially because they share one inference route.

## Sotto — a clean merge can still break the build (`#474`)

Maintainer `Maxerns` verified the fix (and did a negative control: removing the collision check makes the new regression test fail), but blocked on one thing: after merging current `main`, the branch no longer compiled. `#516` had changed `set_status` to take a `StatusKind`, so the new call and the test assertion needed updating (`StatusKind::Failure`, assert on `notice.message`). We rebased, fixed both spots, and reran the TUI suite (57 passing) plus fmt and clippy.

**Lesson:** GitHub's "no conflicts" only means the text merges. Before asking for re-review on a PR that has sat for more than a few days, rebase onto `main` and rebuild and test locally. Signature changes in shared helpers like `set_status` are the usual culprit.

## Status only

- Sotto `#473` merged 2026-10-02.
- NeMo Agent Toolkit `#2226`: `willkill07` merged `develop` into the branch on 2026-10-07 (keeping it current for CI).
- OpenClaw `#158348`: still open, no new review.
- OrcaRouter-Lite, OrcaReplay, Orca-Code-Review, T3, Prime, Hermes, Omnigent, Bridge: no new activity on our PRs since the last note.

## Links

- https://github.com/NVIDIA/NemoClaw/issues/12059
- https://github.com/anomalyco/opencode/issues/36766
- https://github.com/getsotto/sotto/pull/474
- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2226
