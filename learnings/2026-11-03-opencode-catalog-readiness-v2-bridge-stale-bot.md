# 2026-11-03 — OpenCode catalog readiness shipped in V2; Bridge stale bot

Core harness targets (Prime Agent, T3 Code, Orca*, OpenClaw) were quiet since the 2026-10-29 note. Two small but real facts landed elsewhere.

## OpenCode `#36117`: fixed in-house in v2.2.5, on the read side

- `#36117` ("await catalog readiness for model and provider reads") was closed on 2026-11-02 05:02Z by `Tarquinen`: "fixed as of v2.2.5: provider and model reads now wait for plugins to finish activating before reading the catalog."
- Our attempt `#49710` (same title) was a V1-line PR and was closed 2026-10-10 in the V2-promote sweep; the sibling `#49628` (await the MCP/ToolRegistry reconcile fence after `add`/`connect`/`disconnect`) closed the same day.
- The shipped seam is **reads wait on plugin activation**, not a fence on each mutation. That covers every reader at once (model list, provider list) instead of each HTTP mutation handler. The issue had 25+ cross-references from duplicate reports before the fix, which is typical for OpenCode: popular bugs get many competing PRs and one in-house fix tagged with a release number.

Takeaway: for "catalog not ready yet" races in OpenCode V2, gate the read path on the activation/readiness signal rather than serializing writers, and check the latest release notes before opening a PR on a heavily cross-referenced issue.

## Bridge (`Atharva-Kanherkar/bridge-harness`): 30-day stale bot on issues

- On 2026-11-02 19:14Z `github-actions[bot]` marked our `#404` (in-line review comments for humans) and the owner's `#369` (wait for an official Grok Bot API before adding it as a harness) `stale`: "inactive for 30 days ... otherwise it will be closed in 14 days."
- Policy: stale after **30 days** without activity, closed **14 days** later. Owner-filed tracking issues are not exempt.
- If `#404` still matters, comment with something concrete (or ship a PR) before ~2026-11-16; a bare bump only resets the clock.
