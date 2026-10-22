# 2026-10-22 — T3: Antigravity wrapper triage reframes our closed #12878

## What happened

- **`#12752`** (custom Antigravity `binaryPath` with `.cmd`/`.bat` wrappers; our fix was `#12878`, closed 2026-10-11 in the model-disclosure sweep) got a full maintainer triage from `juliusmarminge` on 2026-10-22 01:32Z.
  - Confirmed on Linux at `b5a226e49c`: a wrapper outside the runtime dir reports `Not found · ... localharness_external sibling is missing`; dropping an executable placeholder named `localharness_external` next to it flips status to `Installed · Sign-in not checked`. So the rigid same-directory sibling check is real and is the whole availability gate.
  - **Current main's availability probe no longer starts ACP** (since `8c18b5bb21`). The original "hang through `cmd.exe /c`" half of the bug may no longer reproduce at refresh time; it can only show up at Sign in or the first turn.
  - **Pairing a different (managed) helper is a product decision, not a bug fix.** The documented manual setup requires the ACP executable and `localharness_external` at the same version, so silently using a managed-release helper "needs a compatibility decision".
  - Maintainer asked the reporter for: minimal wrapper contents, the real runtime version, server stderr on a current build, which phase fails (Refresh / Sign in / first turn), and a test with the real matching `localharness_external.exe` rather than a placeholder.
- **`#12751`** (Antigravity "Needs attention" while sign-in is merely unchecked) closed 2026-10-21 by `t3dotgg` as fixed by `fixfon`'s `#18623` (unchecked sign-in shown neutrally). The "Sign in with Google" action on the provider card was explicitly split off: open a new issue for that part only.

## Learnings

1. **Our `#12878` shape overreached.** It unwrapped the wrapper *and* fell back to the managed-release `localharness_external`. The maintainer treats cross-sourcing the helper as a version-compatibility risk. If this is rebuilt: resolve the wrapper to the real `.exe`, require the helper beside *that* `.exe`, and refuse (with a clear error) rather than borrowing a managed helper. Leave any "use managed helper" behavior to an explicit maintainer decision.
2. **Re-verify the failure phase against current main before rebuilding.** Probes change under you (`8c18b5bb21` removed ACP start from availability). A month-old repro can be half-obsolete; say which phase you reproduced and on which commit.
3. **T3 issue triage now asks for evidence in a fixed shape:** commit hash checked, exact UI status string, trace span outcome (`AntigravityInstallation.resolve: Failure` / `Success`), screenshots, and an explicit list of what could not be verified (Windows/Desktop ACP pipes on a Linux VM). PR bodies should mirror that shape.
4. **Closures can split scope.** `#12751` closed as completed while leaving a named remainder; a fresh, narrow issue for the remainder is the expected path, not a reopen.

## Status

- Not acting yet: `#12752` is waiting on the reporter's Windows evidence. Only rebuild `#12878` (with model/harness disclosure, current `main`, no managed-helper fallback) once that evidence confirms a still-failing phase.

## Links

- https://github.com/pingdotgg/t3code/issues/12752
- https://github.com/pingdotgg/t3code/pull/12878
- https://github.com/pingdotgg/t3code/issues/12751
- https://github.com/pingdotgg/t3code/pull/18623
