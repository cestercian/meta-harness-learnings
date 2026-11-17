# 2026-11-17 — NemoClaw closes "Classic" PRs while focusing on V1

## What happened

- `NVIDIA/NemoClaw#12342` (docs: drain the Jira Node probe response so the integration-policy example exits) was opened 2026-09-25, had green CI and two polite nudges (Oct 22, Nov 10 tagging the reviewer), and got no review.
- On 2026-11-16 19:50 UTC `wscurran` closed it unmerged: *"We're focusing development on NemoClaw V1, so we're closing this Classic PR without merging."*
- No code objection: the close is a product-line triage, the same shape as OpenCode's V1→V2 promote closes (2026-10-12 note).

## Learning

- NemoClaw maintainers now label pre-V1 work as **Classic** and close it instead of reviewing it. Default branch is still `main`, so the branch name alone does not tell you whether a change is "Classic".
- Aging NemoClaw PRs that only touch Classic surfaces (docs examples, legacy scripts) are at risk of the same close. Our remaining open one is `#12539` (backup restore `.sandbox` marker, `scripts/backup-workspace.sh`): expect the same outcome unless a maintainer engages.
- Before opening new NemoClaw work: check the latest maintainer comments and issues for what counts as V1, and target V1 surfaces only. More nudges on a Classic PR will not help.
- Pattern across harnesses: when a project announces a new major line (OpenCode V2, NemoClaw V1), open PRs against the old line get closed in bulk. Read the roadmap before you nudge.

