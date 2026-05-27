# Agentic Development Workflow Kit

## Overview

This project is a lightweight AI-assisted development workflow based on markdown-defined agents, reusable skills, supervised implementation loops, QA gates, and human approval points.

I built this workflow to make AI-assisted development more controlled. The goal is not to let an AI tool run freely across a repository. The goal is to structure the work so planning, implementation, testing, documentation, and review happen in a repeatable loop.

## Problem

AI coding tools are useful, but unstructured use creates predictable problems:

- oversized tasks,
- vague acceptance criteria,
- shallow review,
- missing tests,
- documentation drift,
- context loss,
- over-trust in generated code,
- unclear rollback paths.

The workflow addresses this by splitting the development process into small roles and explicit handoffs.

## Goal

Create a repeatable development loop where AI assistants can help with planning, implementation, testing, and documentation without replacing human ownership.

The core idea:

> AI can accelerate execution, but responsibility for judgment, architecture, and final approval stays with the developer.

## Workflow

```text
Project state / blueprint
       ↓
Architect-planner
       ↓
Implementation batch
       ↓
Tester
       ↓
Docs-QA
       ↓
Human review
       ↓
Next batch
```

Each loop is intentionally small. The system is designed to complete one controlled implementation cycle, verify it, update state, and stop.

## Agent Roles

### Architect-Planner

The architect-planner reads the project state, chooses the next implementation batch, defines likely affected files, identifies risks, and writes acceptance criteria.

It does not edit source code.

### Tester

The tester verifies the latest implementation batch. It runs targeted checks first, then broader checks when reasonable. It reports regressions, coverage gaps, performance risks, and security concerns.

It does not edit source code.

### Docs-QA

Docs-QA updates the project documentation after implementation and testing. It records what changed, what was verified, what risks remain, and what should happen next.

It does not rewrite source code unless explicitly instructed.

## Supporting Skills

### dev-loop

Runs one supervised development cycle:

1. plan,
2. implement,
3. test,
4. update documentation/state,
5. stop.

The stop condition matters. It prevents the workflow from drifting into uncontrolled multi-hour changes without review.

### repo-map

Inspects an unfamiliar repository from actual files before writing onboarding or architecture notes.

### agent-communicator

Defines shared request, response, status, and error-log patterns so agents coordinate through explicit artifacts instead of hidden assumptions.

### error-recovery

Defines recoverable, partial-success, and fatal error categories, plus retry, rollback, escalation, and manual recovery behavior.

### performance-monitor

Tracks workflow execution time, success rate, error rate, resource use, and throughput.

## Design Principles

- Keep agent roles narrow.
- Use one implementation batch at a time.
- Define acceptance criteria before editing.
- Verify before updating documentation.
- Log risks and unresolved issues.
- Keep rollback paths visible.
- Treat AI output as draft work until reviewed.
- Preserve human responsibility for final decisions.

## Why This Matters

AI-assisted development is not just prompting. It is a workflow design problem.

This project shows how I think about practical agentic development; not as autonomous magic, but as structured collaboration between tools and a developer who remains accountable for the result.
