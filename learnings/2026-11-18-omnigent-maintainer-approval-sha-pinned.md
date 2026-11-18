# 2026-11-18 — Omnigent "Maintainer Approval" is a SHA-pinned gate

## What happened

- `omnigent-ai/omnigent#8312` (archive inactive sessions after a retention period) was brought up to date on 2026-11-17 17:55 UTC with a merge of upstream `main` (`3103600f`, conflicts resolved to keep `ARCHIVED_BY` labels and retention store methods).
- The full suite went green (DCO, coverage at 90% baseline, browser contract shards, compat smokes against v0.22.0 server/runner/SPA, sqlite benchmark), but **Maintainer Approval** failed after 11s with `awaiting approval from a maintainer`.

## How the gate works (`.github/scripts/maintainer_approval.py`, loaded from the `maintainer` branch)

- Passes right away only for maintainer authors or trusted automation authors.
- For everyone else it needs an `APPROVED` review from a maintainer **whose `commit_id` equals the current head SHA** (`EXPECTED_HEAD`).
- An older approval still counts only if every commit after it was authored, committed **and pushed** by trusted automation (the "trusted successor" path, checked through stale-review-dismissal timeline actors). Commit emails aren't trusted, because anyone with push access can spoof them.

## Learning

- On an outside PR a red **Maintainer Approval** check is the normal state before review. It does not mean CI is broken, so don't "fix" it and don't cite it in a nudge.
- After a maintainer approves, **any push from us (rebase, merge-main, GitHub update-branch) resets the gate**, because our login is not a trusted successor. Once approved, stop updating the branch unless a maintainer asks or there is a real conflict, and then ask them to re-approve the new head.
- Updating before review is fine and keeps the compat smokes meaningful. Just don't treat the update as something that will make the PR mergeable without a human.
- Cross-harness pattern: Omnigent (head-pinned approval), NAT (`/ok to test` per push, 2026-10-16 note) and OpenClaw (bot-driven merges) all tie their merge gate to a human or trusted-bot action on the exact head. Babysit loops should stop pushing once that action lands.
