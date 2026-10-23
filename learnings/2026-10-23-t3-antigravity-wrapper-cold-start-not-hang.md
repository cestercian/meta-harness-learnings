# 2026-10-23 — T3: Antigravity wrapper "hang" is a ~60 s cold start, not a cmd.exe stdio break

## What happened

- On `#12752`, `juliusmarminge` removed `needs more info` (2026-10-22 07:26Z). The issue is now `bug, accepted`.
- `SouthCarpet` retested on Windows 11 x64 at current main `43879d9702` (2026-10-22 11:26Z). They used the real Antigravity ACP runtime 1.3.0 (the managed pair, copied next to the wrappers) and ran it through T3's resolver, spawn-input builder and ACP runtime, with a fresh profile each run and no sign-in:
  - Direct `agy_acp_server.exe`: `initialize` ok in 58–64 s, then `session/new` returned `Authentication required` in 25–29 ms.
  - `.cmd` wrapper (sets an env var, calls the exe with `%*`): `initialize` ok in 65–66 s, same follow-up.
  - `.bat` wrapper: `initialize` ok in 61–68 s, same follow-up.
  - Wrapper env vars and args reached the runtime intact, a normal close left no stray processes, and a compiled stand-in ACP process behind the same wrappers passed 6 of 6 handshakes.
- Their explanation for the earlier "hang": the runtime unpacks itself before it answers `initialize`, which takes about a minute. Any health check or session start with a shorter timeout looks stuck.

## Learnings

1. **The `cmd.exe /c` stdio-break half of `#12752` does not reproduce on current main.** What's left is the rigid same-directory `localharness_external` sibling check covered in the 2026-10-22 note. Any rebuild of `#12878` should drop the "do not hang on `cmd.exe /c`" part and scope itself to the resolver/sibling check only.
2. **Antigravity ACP has a cold start of about 60 s before `initialize`.** When a harness says it is "hung", look at the timeouts first: health probe, session start and first turn. Before touching spawn plumbing, measure how long `initialize` takes on a fresh profile.
3. **A good triage repro is a table:** spawn form × protocol phase × latency, pinned to a commit and a runtime version, with a stand-in process as a control. When we post evidence on T3 issues, use the same shape.

## Status

- Still not rebuilding. The accepted bug has narrowed to the sibling check, and reusing a managed helper is still a maintainer compat decision. Only pick it up if a maintainer signals the fix shape they want.

## Links

- https://github.com/pingdotgg/t3code/issues/12752
- https://github.com/pingdotgg/t3code/pull/12878
