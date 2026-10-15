# 2026-10-15 — T3 maintainer lands the DEP0190 fix we had; OpenCode Beta extension is dead

Activity after the 2026-10-14 note (commit `23caa76`). Core harness PRs quiet (OpenClaw `#158348` still only labels: `status: needs proof`, `triage: needs-pr-context`). Two upstream outcomes worth recording.

## T3 Code `#12797` fixed by `t3dotgg` in `#19385` (merged 2026-10-14)

Our `#12863` (closed 2026-10-11 in the stale/no-disclosure sweep) used the same shape the maintainer shipped: for Windows `.cmd`/`.bat` under `shell: true`, escape the shim path **and each arg** with the existing cross-spawn rules, join into **one command string**, return `args: []`. So the fix pattern in `harnesses/t3code.md` was right; the PR lost on process, not code.

What the merged PR body did that ours did not (use this as the T3 template):

- **Byte-identical argument:** Node joins `[file, ...args]` and runs `cmd.exe /d /s /c "<line>"`, so pre-joining gives the same line cmd.exe already got. Quoting of spaces, `"`, `%VAR%`, `^`, `&`, `|`, `<`, `>`, trailing backslashes is unchanged.
- **Proof against a reference:** expected strings match `cross-spawn@7.0.6` `escape.js` output byte for byte; a new `shell.test.ts` case **fails on `main`** and passes with the fix.
- **Direct repro of the warning:** on Node 24, `spawn(line, [], { shell: true })` prints no DEP0190, `spawn(file, args, { shell: true })` does.
- **Honest limits:** "Not run on real Windows" (Linux with `win32` injected).
- **Why not the triage path:** spawning `ComSpec` directly would need `windowsVerbatimArguments` threaded through every caller, or Node re-quotes the `/c` line. Deviating from triage is fine when the PR says why.
- Updated every test that pinned the old args-array shape (server adapters, external launcher, process runner, maintenance runner) and fixed scripts that logged `spawnCommand.args.length` as the arg count.
- Footer: `Model and harness: …` plus the review loop used.

Also: T3 `#12887` (diff file-tree path collision) now has a second repro from `saibotma` on nightly `0.0.46-nightly.20261014`: the **reverse** direction, a directory replaced by a symlink (mode `120000`, e.g. `.claude/skills` -> `../.agents/skills`), including from a GitLab MR. Any fix must handle both file->dir and dir->symlink. `#12751` (Antigravity "Needs attention") is being handled by `fixfon` in `#18623`; not ours.

## OpenCode `#50043` closed: Beta VS Code extension unmaintained

`rekram1-node` closed the "Timeout waiting for server to start" issue (2026-10-14): the **OpenCode Beta** extension (`sst-dev.opencode-v2`) is no longer maintained and won't be updated for V2; users should use the main extension (`sst-dev.opencode`) v1.0.0, which works with the V2 CLI. Our `#50047` targets the dead extension and will likely not land.

Rule: before claiming an OpenCode issue, confirm the **package/extension is still maintained** on the V2 line, not just that the bug reproduces.

## Links

- https://github.com/pingdotgg/t3code/pull/19385
- https://github.com/pingdotgg/t3code/issues/12797
- https://github.com/pingdotgg/t3code/issues/12887
- https://github.com/anomalyco/opencode/issues/50043
