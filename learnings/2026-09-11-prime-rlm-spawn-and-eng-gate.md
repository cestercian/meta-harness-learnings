# 2026-09-11 — Prime Agent: `rlm.spawn`, one roster, ENG tickets

Overnight upstream (vouched maintainers) tightened the recursive-agent API and widened the Linear gate. Still discussion-first / vouched-only for outsiders — these notes are for when we *are* invited.

## Architecture

### Explicit spawn only (`#2152`)
- Recursive child spawn is now **only** `await rlm.spawn(prompt, *, name=..., model=..., thinking=...)`.
- `rlm.run` removed; calling `rlm(...)` or the `rlm` module raises `TypeError` that names `rlm.spawn`.
- `name` is required at the Python signature (empty/uniqueness still host-side).

### One agent roster (`#2153`)
- Twin lists disagreed: `agent_message.list_agents()` (family catalog / on-disk) vs `agent_observe.list_agents()` (live + passive RLM).
- Roster is now **only** `agent_observe.list_agents()`, built from the same family catalog `agent_message.send` uses — one membership definition for reachability.

## Contrib posture

### Linear ticket gate accepts ENG + RES (`#2124`, `linear-ticket.yml`)
- PR title / body / branch must match `\b(?:eng|res)-\d+\b` **or** include `No-Ticket: <reason>`.
- ENG → Engineering team / Prime Agent V1 board; RES → Research / Long-Horizon board.
- Vouch gate unchanged (`contribution-gate.yml` + mitchellh/vouch still auto-closes unvouched issues/PRs).

## Links
- https://github.com/PrimeIntellect-ai/prime-agent/pull/2152
- https://github.com/PrimeIntellect-ai/prime-agent/pull/2153
- https://github.com/PrimeIntellect-ai/prime-agent/pull/2124