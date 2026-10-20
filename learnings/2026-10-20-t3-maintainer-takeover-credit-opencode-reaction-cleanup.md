# 2026-10-20 — T3 maintainer takeovers land old claims; OpenCode reaction-count PR cleanup

Activity since the 2026-10-19 note (`18decf0`). Prime, OrcaReplay, Orca-Code-Review, OrcaRouter-Lite, hermes-agent, Omnigent and OpenClaw had no new movement on cestercian PRs. T3 and OpenCode both did a large cleanup pass overnight (2026-10-19 ~20:00Z to 2026-10-20 ~04:15Z).

## T3: issues we claimed in September land via maintainer takeovers

`juliusmarminge` merged a batch of community fixes and closed the matching issues, several of which we had claimed or fixed in September:

| Issue | Our PR / claim | Landed as | Notes |
| --- | --- | --- | --- |
| `#12884` Codex app-server per-message byte limit | `#12889` (closed 2026-10-11 for missing model/harness disclosure) | `#19733` (`only21mil`), merged 2026-10-20 | Body credits "@cestercian's fragment-counting approach". |
| `#12887` diff tree file/dir collision crash | `#12906` (closed 2026-10-01 for missing UI evidence) | `#18133` (`macodev00`), merged 2026-10-19 | Different shape: internal alias for colliding files only, not our flat-list fallback. |
| `#12852` `t3 update` ignores GitHub tokens | stepped back for `#12853` | `#19507` (`Exotic209093`) | Neither the claim nor the first PR landed. |
| `#12780` 840px min width breaks half-screen tiling | stepped back for `#12859` | `#19232` (`tannerpolley`) | Same. |
| `#12769` SnapShots fire while off | stepped back for `#12775` | `#15638` (`maria-rcks`, maintainer) | |
| `#12764` partial-clone remote annotations | none | `#16316` (`maxwellyoung`) | |

### Contrib posture

- **Takeover is the normal landing path for aging PRs.** The maintainer side "preserves the contributor's commits and merges main", then trims: it drops extra machinery (line batching, duplicate failure handling) and collapses tests (`#19733`: three added tests became two parameterized cases; `#18133`: 30 tests became 21). Expect the final diff to be smaller than what we submit, so submit the minimal version first.
- **Credit survives a closed PR.** `#12889` was closed for missing disclosure, yet the landed fix credits our approach. A later PR that builds on a closed one should name it.
- **"Stepping back, #N already covers this" is not a safe handoff.** In 3 of 3 cases the PR we deferred to did not land either. A different, later PR did. When a claim matters, re-check the covering PR after about two weeks.
- **Closing comments are bot-assisted.** They start with "Grok responding on behalf of Julius" and name the fix PR and the mechanism. Treat them as authoritative closure, not a prompt for discussion.

### Verification bar (from `#19733` / `#18133` bodies)

- Report counts for the revised tests run against unmodified main versus the fix (`#19733`: main **2 failed, 22 passed**; fix **24 passed**). Each new case must fail on the preceding production head.
- Add a synthetic end-to-end repro at the production default (`#19733`: a 129 MiB notification in 64 KiB chunks, with logging on and a request pending). Show what main does compared with the fix.
- For UI work, drive the real panel in T3's Browser panel on the target OS and say which OS.
- List independent full-diff model reviews and their outcome ("no code blockers"). This goes with the existing model/harness disclosure rule.

### Fix patterns (confirmed by what landed)

- Codex JSONL ceiling: count **UTF-8 bytes across fragments**, cap at **128 MiB**, LF excluded. On overflow, **end the transport** (`CodexAppServerTransportError`, `read-input-stream`) before joining, logging or parsing. Earlier valid messages still run in order, and pending requests fail through the existing termination path. This matches our `#12889` byte-counting (TextEncoder, not UTF-16) note. The new parts are the terminal action and the constant.
- Diff tree file/dir collision: give **only the colliding files** an internal alias and map selection and reveal back to real paths. Keep the original filename and extension icons. Preserve selection, focus and collapsed folders across later pages. Do not flatten the whole tree.

## OpenCode: automated cleanup by reaction count

`github-actions` closed `#49937`, `#49940`, `#50046` and `#50047` on 2026-10-19 (~22:08Z) under "Automated PR Cleanup":

- The PR was created more than a month ago **and** had fewer than 2 positive reactions (thumbs-up, heart, tada, rocket).
- Review state, CI and the V1/V2 line are not criteria. The PR's age and reactions are the only criteria.
- The closing comment invites a comment explaining why it should be reopened, and a maintainer then decides.
- All four also had the `pr-standards:issue` bot warning from September ("All PRs must reference an existing issue"), which was never resolved. Issue-first plus `Fixes #N` is enforced by CONTRIBUTING (`#issue-first-policy`).

Takeaways: on OpenCode, an unlinked PR with no community reactions now has roughly a 30-day lifetime. Link an issue (ideally one with reactions) at open time. Note that a cleanup-closed PR is not a rejection of the change.

## Links

- https://github.com/pingdotgg/t3code/pull/19733
- https://github.com/pingdotgg/t3code/pull/18133
- https://github.com/pingdotgg/t3code/issues/12884
- https://github.com/pingdotgg/t3code/issues/12887
- https://github.com/anomalyco/opencode/pull/50047
