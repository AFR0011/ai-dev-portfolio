---
name: dev-loop
description: Run one supervised development loop cycle from BLUEPRINT.md. Calls repo-map if required docs are missing.
---

Read:
- BLUEPRINT.md
- DEV_STATE.md
- DEV_LOG.md
- QA_REPORT.md
- RISK_REGISTER.md

## Pre-flight: Check Required Docs

Before running the cycle, verify all required docs exist:
- If **BLUEPRINT.md** is missing: Run `repo-map` with `lean` pack to create it along with AGENTS.md and docs/PROJECT_STATE.md
- If **DEV_STATE.md** is missing: Run `repo-map` to generate it
- If **DEV_LOG.md**, **QA_REPORT.md**, or **RISK_REGISTER.md** are missing: Run `repo-map` with `standard` pack

## Shared Context Protocol

Before each cycle, check for:
- Any ongoing processes or locks on key files
- Recent changes that might conflict with current planning
- Agent execution history to avoid redundant operations

## Run exactly one cycle:

1. Use architect-planner to produce the next active batch.
2. Executor implements only that batch.
3. Use tester to verify it.
4. Use docs-qa to update documentation, state, logs, and risks.

Stop after one cycle.

Do not continue automatically unless the external orchestrator calls again.

## Rollback Capability

If tester returns FAIL:
- Log the failure with timestamp and agent details
- Attempt automatic rollback of changes
- Document rollback steps in DEV_LOG.md
- Escalate to human review if rollback fails

## Audit Trail

All agent interactions are logged with:
- Agent name and timestamp
- File modifications made
- Reason for changes
- Execution status (success/failure)

## Integrated Skills

This workflow now integrates with the following enhanced skills:
- **agent-communicator**: Standardized communication protocol between agents
- **error-recovery**: Handles agent failures gracefully with rollback operations
- **performance-monitor**: Tracks and analyzes performance metrics