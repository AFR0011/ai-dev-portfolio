---
name: agent-communicator
description: Standardized communication protocol for agents in the dev-loop workflow
---

# Agent Communication Protocol

## Overview
This skill provides a standardized communication mechanism for all agents in the dev-loop workflow to share status, request information, and coordinate operations.

## Communication Methods

### 1. Shared Status Queue
All agents must:
- Write status updates to `shared/status.md` before and after major operations
- Include timestamp, agent name, operation type, and result
- Use consistent formatting for status entries

### 2. Request/Response Protocol
Agents can:
- Request information from other agents by writing to `shared/requests.md`
- Respond to requests in `shared/responses.md`
- Include request ID, requested data type, and deadline

### 3. Error Notification
When agents encounter issues:
- Write error details to `shared/errors.md`
- Include error type, severity, affected components, and suggested actions
- Use standardized error codes for categorization

## Agent Responsibilities

### Status Updates
Each agent must:
- Log start of operation with timestamp
- Log completion or failure with timestamp
- Include brief operation summary
- Indicate resource usage if applicable

### Request Handling
When receiving requests:
- Process within deadline or escalate if not possible
- Return appropriate response or error
- Log request handling time

### Error Handling
When encountering errors:
- Log error details immediately
- Attempt automatic recovery if possible
- Escalate to human review if recovery fails
- Update shared error log

## File Formats

### Status File (`shared/status.md`)
```
[timestamp] [agent-name] [operation-type] [status] [details]
```

### Request File (`shared/requests.md`)  
```
[request-id] [requesting-agent] [request-type] [deadline] [details]
```

### Response File (`shared/responses.md`)
```
[request-id] [responding-agent] [response-type] [timestamp] [data]
```

### Error Log (`shared/errors.md`)
```
[timestamp] [agent-name] [error-type] [severity] [description] [suggested-actions]
```

## Protocol Rules

1. **All files are in shared directory** - Agents must not create local communication files
2. **Timestamps are ISO 8601 format** - Ensures consistency across systems
3. **Error codes are standardized** - See error code reference section
4. **Requests have deadlines** - Prevents indefinite waiting
5. **Responses are required** - All requests must be responded to

## Error Code Reference

- **E001**: Resource unavailable
- **E002**: Permission denied  
- **E003**: Data inconsistency
- **E004**: Timeout
- **E005**: System overload
- **E006**: Configuration error
- **E007**: Tool failure
- **E008**: Validation error

## Implementation Requirements

All agents must implement:
- Status logging before/after operations
- Request handling protocol
- Error reporting and escalation
- Shared file access patterns