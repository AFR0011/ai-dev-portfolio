---
name: performance-monitor
description: Tracks and analyzes performance of agents in the dev-loop workflow
---

# Performance Monitoring Protocol

## Overview
This skill provides comprehensive performance tracking and analytics for all agents in the dev-loop workflow, enabling optimization and bottleneck identification.

## Performance Metrics Collected

### 1. Execution Time Metrics
- **Agent execution time**: Total time for each agent operation
- **Batch processing time**: Time to complete entire batch cycle
- **File I/O time**: Time spent reading/writing files
- **Tool execution time**: Time spent running external tools

### 2. Resource Usage Metrics
- **CPU utilization**: Percentage of CPU used by agent operations
- **Memory consumption**: RAM usage during operations
- **Disk I/O**: Read/write operations and throughput
- **Network usage**: Network requests and bandwidth

### 3. Workflow Efficiency Metrics
- **Success rate**: Percentage of successful operations
- **Error rate**: Percentage of failed operations
- **Recovery time**: Time to recover from errors
- **Throughput**: Number of operations completed per time unit

## Data Collection Methods

### 1. Automatic Timing
- All agents must record start/end timestamps for major operations
- Timing data is automatically collected and stored
- Granular timing for different operation phases

### 2. Resource Monitoring
- System resource usage is monitored during agent execution
- Resource data is collected at regular intervals
- Peak usage values are recorded

### 3. Workflow Tracking
- End-to-end workflow timing is tracked
- Bottleneck identification through time analysis
- Performance trend analysis over time

## Data Storage

### Performance Log Format (`shared/performance.md`)
```
[timestamp] [agent-name] [operation-type] [execution-time] [resource-usage] [success/failure] [notes]
```

### Trend Analysis (`shared/trends.md`)
```
[timestamp] [metric-name] [value] [trend-direction] [confidence-level]
```

### Bottleneck Reports (`shared/bottlenecks.md`)
```
[timestamp] [bottleneck-type] [affected-agent] [impact-level] [recommendation]
```

## Analysis Capabilities

### 1. Real-time Monitoring
- Live performance tracking during execution
- Immediate alerts for performance degradation
- Dynamic resource allocation recommendations

### 2. Historical Analysis
- Performance trend identification
- Bottleneck pattern recognition
- Optimization opportunity detection

### 3. Predictive Analysis
- Future performance predictions
- Resource requirement forecasting
- Workflow optimization recommendations

## Alerting System

### Performance Thresholds
- **Warning**: Performance degrades by 20% from baseline
- **Critical**: Performance degrades by 50% from baseline
- **Severe**: Performance degrades by 80% from baseline

### Alert Types
- **Execution time alerts**: Operations taking longer than expected
- **Resource usage alerts**: High CPU/Memory usage
- **Success rate alerts**: Declining success rates
- **Bottleneck alerts**: Identified performance bottlenecks

## Optimization Recommendations

### 1. Agent-level Improvements
- Suggest code optimizations for slow agents
- Recommend tool usage improvements
- Propose alternative approaches for bottlenecks

### 2. System-level Improvements
- Resource allocation recommendations
- Workflow restructuring suggestions
- Infrastructure optimization tips

### 3. Process Improvements
- Batch size recommendations
- Parallelization opportunities
- Caching strategy improvements

## Integration with Dev-Loop

### Real-time Feedback
- Performance data is available during workflow execution
- Agents can adjust behavior based on performance metrics
- Dynamic workflow optimization

### Post-execution Analysis
- Comprehensive performance report after each cycle
- Trend analysis over multiple executions
- Optimization suggestions for next cycle

## Implementation Requirements

All agents must:
- Record execution time for major operations
- Log resource usage where possible
- Report performance data to shared storage
- Follow performance monitoring protocols
- Implement optimization recommendations when provided

## Data Privacy and Security

### Data Protection
- Performance data is anonymized
- No sensitive information is stored
- Data is only used for optimization purposes
- Access controls are implemented for performance data

### Compliance
- Follow data privacy regulations
- Ensure data retention policies are followed
- Provide data export capabilities when requested