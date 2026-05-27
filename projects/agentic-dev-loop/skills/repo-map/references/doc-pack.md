# Repo Mapping Doc Pack

## Lean Pack
- `AGENTS.md`
- `docs/PROJECT_STATE.md`
- `docs/REPO_MAP.md`

Use when the repo only needs enough structure to onboard work quickly.

## Standard Pack
- Lean pack
- `docs/RUN_PROTOCOL.md`
- `docs/VERSION_LOG.md`

Use when verification expectations or milestone tracking matter.

## Extended Pack
- Standard pack
- repo-specific docs such as:
  - `docs/LESSONS.md`
  - `docs/MIGRATION_BACKLOG.md`
  - another tightly scoped operational note

Use when the repo is being migrated, stabilized, or adopted as a long-lived workspace.

## Selection Rules
- Prefer the smallest pack that still prevents repeated confusion.
- Add `docs/LESSONS.md` only when recurring traps are likely.
- Add `docs/MIGRATION_BACKLOG.md` when the mapping pass consolidates TODOs from multiple historical notes.
- Do not add generic filler docs such as repo-internal READMEs just because a template could support them.
