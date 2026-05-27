---
name: repo-map
description: Inspect a newly adopted repository, infer its real project shape, and create or update onboarding docs and repo-local Codex config from repo evidence. Use when the user asks to map a repository, onboard a project, set up AGENTS/docs/config for a codebase, or start work in a repo whose structure, workflows, and source-of-truth files are not yet documented.
---

# Repo Map

## Overview

Map the repository from on-disk evidence first, then create or update the requested operating docs without inventing architecture, workflows, or verification steps that the repo does not support.

## Workflow

### 1. Inspect Before Writing

- Identify the real repo type from the filesystem:
  - framework app
  - legacy website
  - library/package
  - research workspace
  - scripts/data workspace
- Check for existing source-of-truth files before asking questions:
  - `AGENTS.md`
  - `README*`
  - `docs/`
  - schema/config files
  - manifests such as `package.json`, `composer.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`
- Record the actual runtime/build signals the repo shows:
  - languages present
  - package managers
  - test/build tools
  - DB dumps or migrations
  - deployment clues

### 2. Infer The Minimum Safe Doc Pack

- Respect the requested pack when the user specifies one.
- Default packs:
  - `lean`: `AGENTS.md`, `docs/PROJECT_STATE.md`, `docs/REPO_MAP.md`
  - `standard`: lean pack plus `docs/RUN_PROTOCOL.md` and `docs/VERSION_LOG.md`
  - `extended`: standard pack plus repo-specific docs such as `docs/LESSONS.md`, `docs/MIGRATION_BACKLOG.md`, or another clearly justified operations note
- Only add docs that materially help future work.

Detailed pack guidance lives in [references/doc-pack.md](references/doc-pack.md).

### 3. Write From Evidence, Not Habit

- Name the real source-of-truth paths for the repo.
- Document the real verification model:
  - automated tests if they exist
  - manual/browser/DB checks if that is what the repo actually supports
- Call out current drift explicitly:
  - duplicated logic
  - mixed config sources
  - missing environment manifests
  - legacy pages/scripts still in use
- If the repo lacks history or version control, say so plainly.

### 4. Prefer Small, Reusable Operating Docs

- `AGENTS.md`: repo type, source of truth, operating rules, verification rules, done criteria
- `docs/PROJECT_STATE.md`: current snapshot, active objective, risks, verification state
- `docs/REPO_MAP.md`: structure, ownership, entrypoints, data flow, active vs legacy surfaces
- `docs/RUN_PROTOCOL.md`: verification ladder and local execution notes
- `docs/VERSION_LOG.md`: meaningful milestones only
- Optional docs only when the repo benefits from them:
  - `docs/LESSONS.md`
  - `docs/MIGRATION_BACKLOG.md`
  - repo-specific operational notes

### 5. Config Handling

- Create repo-local Codex config only when the user asks for it or when the mapping task explicitly includes it.
- Match existing local patterns when the user already uses a standard template across repos.
- Do not invent aggressive permissions or network access defaults.

### 6. Final Output Requirements

- Leave the repo with:
  - created or updated docs
  - a clear state snapshot
  - a repo map grounded in actual files
  - a realistic verification path
  - explicit remaining risks and next actions
- If runtime verification is blocked by environment limits, document the exact blocker instead of claiming full validation.

## Special Notes

- Treat historical scratch logs like `progress.md` as context, not canonical truth, unless the repo clearly says otherwise.
- Keep repo-specific policy in the repo docs. Do not bake project-specific assumptions into this skill.
