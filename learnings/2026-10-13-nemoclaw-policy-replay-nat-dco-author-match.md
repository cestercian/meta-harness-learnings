# 2026-10-13 — NemoClaw policy-remove replay, NAT DCO author/sign-off mismatch

Activity after the 2026-10-12 note (commit `8873483`). One more NemoClaw maintainer replay confirms the salvage pattern, and NAT `#2226` hit a DCO gotcha caused by our commit-signing setup.

## NemoClaw `#12111` → `#12983` (merged, co-author credit)

`ericksoa` replayed our policy-remove fix as `#12983` (`fix(policy): verify preset removal against operator-owned rules`), merged 2026-10-12 with `Co-authored-by: Cestercian`, then closed `#12111` and issue `#12048`. The replay PR body said "keep the original open until this replay lands", so the original is closed only after the replacement merges.

**Architecture insight:** NemoClaw already strips reserved `_provider_*` entries *before* policy writes, but compared the full OpenShell readback *after*. The fix compares through the **same provider-entry stripping boundary** used on write, rather than a one-off key strip at compare time, and still fails verification when any operator-owned rule is missing or changed (including an unrelated host grant disappearing). Our regression case was kept as-is.

**Pattern (now 2 for 2 this week, with `#12110` → `#12968`):** for a correct-but-aging NemoClaw PR, the likely landing path is a maintainer replay onto current `main` with co-author credit plus full Docker/Podman E2E fanout against an exact baseline ("do-no-harm"). Keep the regression test sharp and minimal, because that is the part that survives the replay. Do not re-push or reopen the original after the close comment.

## NAT `#2226` — DCO needs commit author == `Signed-off-by`

After the review follow-up push, DCO went red: commit `ce6fda87` had author `yashafaid@gmail.com` (from the GPG signing setup) but `Signed-off-by` used the `users.noreply.github.com` address. Reviewer `Somay-kousis` pointed out the fix: amend the author to the signed-off address and force-push. Fixed 2026-10-13, and DCO is green.

**CI gotcha:** on any DCO repo (NAT, Sotto), the author email and the sign-off email must match exactly. When the GPG init script sets the committer/author to `yashafaid@gmail.com`, sign off with that same address (`git commit -s` after setting `user.email`), or set both to the noreply address. Do not mix them.

**Design note from the same review:** the streaming retry guard can't tell an exhausted one-shot iterator apart from a retry that legitimately yields nothing. We kept the re-raise, documented it as a "Streaming caveat" in the `patch_with_retry` docstring, and pinned the 429-then-empty case with a test. The reviewer accepted that tradeoff, and the PR is waiting on maintainer `/ok to test` and merge.

## Links

- https://github.com/NVIDIA/NemoClaw/pull/12983
- https://github.com/NVIDIA/NemoClaw/pull/12111
- https://github.com/NVIDIA/NemoClaw/issues/12048
- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2226
