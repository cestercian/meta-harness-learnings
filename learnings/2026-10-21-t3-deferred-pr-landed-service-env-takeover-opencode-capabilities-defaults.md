# 2026-10-21 — T3: a deferred-to PR did land; service.env lands without our credit. OpenCode fixes the broader capabilities bug in a release

Activity since the 2026-10-20 note (`learnings/2026-10-20-*`). Prime, OrcaReplay, Orca-Code-Review, OrcaRouter-Lite, hermes-agent, Omnigent and OpenClaw had no new movement on cestercian PRs. Three more closures from the same overnight T3/OpenCode cleanup pass (2026-10-20 ~05:00Z–09:30Z) landed after yesterday's note was written.

## T3 `#12767`: the PR we stepped back for did land (correction)

- `#12767` (direct desktop builds ignore `--build-version`). We claimed it 2026-09-21, then posted "stepping back — #12768 already covers this" six minutes later.
- `#12768` (`rohan-patnaik`, opened 2026-09-20, same minute as the cross-reference) was merged by `juliusmarminge` on 2026-10-20 ~05:08Z, a month later. Closing comment is the usual "Grok responding on behalf of Julius" note naming the fix.
- The merged diff grew well past the original fix: the resolved artifact version reaches compilation through `APP_VERSION`, manifests changed by alignment are restored afterward, and an atomic checkout lock covers compile/stage/package so concurrent artifact builds cannot mix versions.

**Correction to 2026-10-20:** deferring is now 1 of 4, not 0 of 3. The one that held had these traits: the covering PR was opened by the issue reporter at filing time, the maintainer confirmed the diagnosis on the issue the same day, and the PR was the only cross-reference. Deferring to a PR like that is reasonable. Deferring to a later drive-by PR is not; keep the two-week re-check either way.

## T3 `#12626`: our service.env approach lands as someone else's takeover, uncredited

- Our `#12633` ("merge T3-home service.env into the background launcher") followed the fix path triage named on the issue, then was closed 2026-10-11 in the missing model/harness disclosure sweep.
- `#19486` (`Exotic209093`, opened 2026-10-14) was merged 2026-10-20 ~05:06Z as a maintainer takeover. Same design: optional `<T3CODE_HOME>/service.env`, read once by the launcher with Node's literal env parser, one merged snapshot passed to active/trial/replacement children, launcher-owned variables protected, missing file harmless, other read errors fail startup, changes need `t3 service restart`, installer leaves the file alone, docs updated.
- The body credits `Exotic209093`'s approach only. Unlike `#19733` (which credited our closed `#12889`), credit is not automatic: it follows whoever's PR is open when the maintainer picks the issue up.
- Second time `Exotic209093` landed an issue we had claimed (`#12852` via `#19507` yesterday).

Takeaway: on T3, a PR closed for a fixable metadata reason (disclosure) loses the issue to the next open PR within days. If the change still matters, reopen as a fresh PR with disclosure the same week instead of letting the claim sit.

## OpenCode `#49912`: fixed in release, broader than our PR

- Our `#49940` ("default omitted capability tools for custom providers") was closed 2026-10-19 by the reaction-count cleanup.
- On 2026-10-20 `neriousy` closed `#49912` as fixed in **2.0.22**: custom providers are no longer skipped when model capabilities omit **`tools`, `input`, or `output`**; omitted fields inherit the model's defaults.
- A commenter (`McNopper`, 2026-09-20) had already reported that *any* incomplete `capabilities` object drops the provider, not only a missing `tools`. Our PR stayed tools-only.

Takeaway: when a later comment widens the trigger, widen the PR (or say why not) before it ages out. On OpenCode, maintainers often fix in-house and close the issue with the release number, so a narrow community PR has little chance even before the 30-day cleanup.

## Links

- https://github.com/pingdotgg/t3code/issues/12767
- https://github.com/pingdotgg/t3code/pull/12768
- https://github.com/pingdotgg/t3code/issues/12626
- https://github.com/pingdotgg/t3code/pull/19486
- https://github.com/pingdotgg/t3code/pull/12633
- https://github.com/anomalyco/opencode/issues/49912
- https://github.com/anomalyco/opencode/pull/49940
