# 2026-11-26 — Hermes Kanban workspace cleanup: reuse the board identity rule, stay fail-closed

**Harness:** NousResearch/hermes-agent
**PR:** https://github.com/NousResearch/hermes-agent/pull/160077 (fixes #160064), opened 2026-11-25, head `beb683c9`, no maintainer review yet

## What broke

Workspace cleanup listed every directory under `boards/` and opened each `kanban.db` read-only to check who owns a workspace.
An empty `kanban.db` in the reserved `boards/_archived/` directory, or a db-only stub dir, has no `tasks` table.
The scan returned `unknown` and cleanup deferred forever.

## Fix pattern

- Don't write a second "is this a board?" check. `_sibling_board_db_files()` now uses the same rule as `list_boards`: a valid slug plus `board.json`, through `_dir_holds_board`.
  Two scanners that disagree about what counts as a board is the bug class.
- Keep the fail-closed path. A real board whose db can't be read is still opened, so cleanup still defers in that case.
  Narrow *which* directories count. Don't make read errors quiet.
- Prove it with a test that fails first: before the fix, the new test fails with `no such table: tasks` and deferral reason `unknown`. All 38 kanban tests pass after.

## Follow-up the reporter flagged (pre-existing, not in this PR)

simoesp (the issue author) reviewed the PR within the hour.
`_sibling_board_db_files()` still skips a candidate when `path.is_file()` is false.
An active board with a missing or unreadable db could be left out of the ownership check.
Any fix must tell active boards apart from archived tombstones. Track it separately or add a test; don't grow #160077.

## Takeaway

In harnesses with several on-disk registries (boards, profiles, sessions), find the canonical identity predicate and reuse it in every scanner.
A directory that "looks like" a board isn't one unless the canonical lister says so.
