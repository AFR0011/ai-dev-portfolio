---
name: error-recovery
description: Handles agent failures gracefully and manages rollback operations
---

# Error Recovery Protocol

## Overview
This skill provides comprehensive error handling and recovery mechanisms for the dev-loop workflow, ensuring that agent failures don't halt the entire process.

## Error Categories

### Recoverable Errors (Retryable)
- **E001**: Resource unavailable (e.g., temporary file lock)
- **E004**: Timeout (e.g., network timeout, tool timeout)
- **E007**: Tool failure (e.g., command execution failure)
- **E008**: Validation error (e.g., input validation failure)

### Partial Success Errors
- **E002**: Permission denied (some operations succeeded)
- **E003**: Data inconsistency (partial data processing)
- **E006**: Configuration error (partial configuration applied)

### Fatal Errors (Non-recoverable)
- **E005**: System overload (resource exhaustion)
- **E010**: Critical infrastructure failure
- **E011**: Data corruption (irreversible data loss)

## Recovery Strategies

### 1. Automatic Recovery
For recoverable errors:
- Retry operation with exponential backoff
- Apply fallback mechanisms
- Use cached data when available
- Attempt alternative approaches

### 2. Manual Intervention Required
For partial success or fatal errors:
- Log detailed error information
- Document recovery steps needed
- Escalate to human review
- Provide clear instructions for manual resolution

### 3. Rollback Operations
When rollback is required:
- Identify affected files and changes
- Restore previous state where possible
- Document rollback actions taken
- Update error logs with rollback status

## Error Handling Process

### Step 1: Error Detection
- Agents detect error conditions
- Error code and severity are determined
- Affected components are identified

### Step 2: Error Classification
- Categorize error type (recoverable, partial, fatal)
- Determine recovery strategy
- Assess impact on workflow

### Step 3: Recovery Execution
- Apply appropriate recovery mechanism
- Update shared error logs
- Notify relevant agents if needed

### Step 4: Status Reporting
- Report recovery status to shared queue
- Update documentation if needed
- Determine next workflow phase

## Agent Integration Requirements

All agents must:
- Implement error detection and classification
- Use standardized error codes
- Log errors to shared error log
- Follow recovery protocols based on error type
- Report status changes to shared status queue

## Recovery Procedures

### For Recoverable Errors:
1. Log error details
2. Wait with exponential backoff (1s, 2s, 4s, 8s...)
3. Retry operation
4. If retry fails, escalate to manual intervention

### For Partial Success Errors:
1. Document partial success
2. Attempt to complete remaining operations
3. Log recovery steps taken
4. Escalate to human review if needed

### For Fatal Errors:
1. Log comprehensive error details
2. Attempt emergency rollback if possible
3. Document all actions taken
4. Escalate to human review immediately
5. Stop workflow execution

## Shared Error Log Format

```
[timestamp] [agent-name] [error-code] [severity] [description] [suggested-actions] [recovery-status]
```

## Recovery Status Values
- **PENDING**: Recovery operation in progress
- **SUCCESS**: Recovery completed successfully
- **FAILED**: Recovery attempt failed
- **ESCALATED**: Requires human intervention
- **ABORTED**: Recovery operation aborted

## Integration with Dev-Loop

When an agent encounters an error:
1. Follow error handling protocol
2. Update shared status and error logs
3. Determine if workflow should continue or stop
4. If workflow continues, proceed to next phase
5. If workflow stops, provide clear recovery instructions

## Testing and Validation

Recovery mechanisms must be tested with:
- Simulated error conditions
- Rollback scenarios
- Recovery success/failure cases
- Integration with other agents