
# Windows Authentication Investigation

This project documents the investigation of multiple failed Windows network authentication attempts observed against a single user account.

## Investigation Summary

The investigation focuses on Event ID 4625 logon failures generated through SMB authentication activity. Evidence was collected from Windows Security logs and analyzed using Splunk.

Key Findings:
- Multiple failed authentication attempts were observed against the account `victim`.
- All observed network logon attempts originated from `192.168.1.4`.
- Authentication was performed using NTLM.
- Logon Type 3 indicated network-based authentication attempts.
- Repeated failures occurred within a short time window.
- The activity pattern was consistent with potential brute-force or automated password-guessing behavior.

## Project Structure

### Report

Contains the complete investigation report, including:
- Scenario
- Evidence
- Analysis
- Verdict
- Recommended Actions

### Screenshots

Contains supporting evidence, including:
- Windows Event Viewer logs
- Event ID 4625 records
- Splunk investigation queries and results
- SMB authentication activity generated during the lab

## Skills Demonstrated

- Windows Event Log Analysis
- Authentication Investigation
- Splunk SIEM Investigation
- NTLM Authentication Analysis
- Evidence Correlation
- Security Reporting
