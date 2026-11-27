# 2026-11-27 — Bridge completion proof: hash the checkout, don't trust git status

**Harness:** Atharva-Kanherkar/bridge-harness
**Issue:** https://github.com/Atharva-Kanherkar/bridge-harness/issues/543 (our backend audit P1, filed 2026-09-21)
**Fix:** https://github.com/Atharva-Kanherkar/bridge-harness/pull/1170 by AyushRajSinghParihar, merged by Atharva-Kanherkar 2026-11-26 (`1950852b`, +1375/-337, 9 files)

## The bug class

Completion proof and waivers were keyed to Git porcelain status.
Porcelain output stays identical when you edit a path that is already modified, staged, or untracked.
So a verifier could pass, the agent could edit the same file again, and Bridge would still credit the old proof.

## Fix pattern

- A fresh, uncached, versioned SHA-256 stamp (`completion/repository_stamp.rs`) over HEAD, index entries, and materialized tracked plus non-ignored untracked files.
- One measurement used everywhere: completion plans, checks, waivers, and worker results. Verifier results are pinned to the attempt they ran against.
- Verifiers check the stamp at launch and at settlement. Adoption compares the integrated bytes against the verified checkout.
- Stamp changed means supersede the attempt. Stamp unavailable means report the measurement failure, never silently credit.
- Submodules: an edit after the child scan invalidates the whole measurement (they reproduced that race on the old code first).
- Migration 101 adds a nullable binding. Old proof stays readable, but reusing a legacy stamp needs fresh verification.

## Posture note

The contributor posted the plan on the issue (with a flow diagram) a day before the PR, then linked it from the PR body.
Plan-on-issue then PR is how outside fixes land smoothly here. Our audit issues are still worth filing: this one sat ~2 months and then got picked up and merged with no pushback.

## Takeaway

Any harness that gates "done" on evidence (tests passed, reviewer approved) needs the evidence bound to content, not to a status summary.
`git status`, mtimes, and "dirty paths" lists are all lossy. Hash what the verifier actually saw and re-check at settle and adopt time.
