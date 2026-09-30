# ATLAS (inferstep)

- **Repo:** https://github.com/inferstep/ATLAS
- **Role:** Adaptive Test-time Learning and Autonomous Specialization — local-first coding-agent / eval harness with Docker graders.
- **Contribution posture (2026-09):** Targets `dev` → staging → main; conventional-commit titles with component scope.

## Notes for contributors

- Eval driver: resolve suite roots to absolute paths at `load_suite` before any `docker run -v` mount. Relative host paths become named volumes and fail (`#276` merged).
- Relative and absolute paths to the same suite must hand the grader the same absolute mount.

## See also

- `learnings/2026-09-30-bridge-dev-updater-mcp-locale-atlas.md`
