# Suspicious Process Investigation

This project focuses on investigating Windows process creation events to distinguish legitimate system activity from behavior that may initially appear suspicious.

## Overview

Process creation events can provide valuable insight into system activity. However, not every unusual process execution indicates malicious behavior. The purpose of this investigation project is to analyze process activity, validate evidence, and determine whether the observed behavior is expected or requires further investigation.

## Investigation Areas

### Baseline

Baseline investigations are used to understand normal Windows process behavior and establish a reference for comparison.

Examples include:

- Windows service activity
- Expected parent-child process relationships
- Legitimate operating system processes
- Trusted and signed executables

### Suspicious

This section contains investigations of activities that may appear suspicious and require detailed analysis before conclusions can be made.

Examples include:

- Unusual process execution
- Unexpected command-line arguments
- Suspicious parent-child relationships
- Living-off-the-Land (LOLBins) activity
- Rundll32-based execution scenarios

## Methodology

Each investigation follows an evidence-based approach:

1. Investigation Scope
2. Evidence Collection
3. Analysis
4. Verdict
5. Recommended Actions

## Directory Structure

```text
Suspicious Process Investigation/
│
├── Baseline/
│
└── Suspicious/
    ├── Basic Execution/
    └── Rundll Execution/
```

## Objective

The goal of this project is to improve process investigation skills, strengthen analytical thinking, and develop a structured methodology for identifying and validating suspicious process activity within a SIEM environment.