# Windows Authentication Investigation

## Scenario

An internal IP address generated multiple failed network authentication attempts against the same account within a short time window.

## Evidence

- **Event ID:** 4625
- **Account Name:** `victim`
- **Source IP Address:** `192.168.1.4`
- **Logon Type:** 3
- **Logon Process Name:** `NtLmSsp`
- **Authentication Package:** NTLM
- **SubStatus:** `0xC000006A`
- **Source Port:** Varying source ports
- **Timestamp:** 12:52:04 PM

Ten Event ID 4625 events were recorded with the same displayed timestamp.

## Analysis

The events originated from the same source IP address and targeted the same account. The varying source ports are consistent with separate network connections.

The Logon Type 3 indicates network logons, while NTLM was used as the authentication package.

The SubStatus code `0xC000006A` indicates an incorrect password.

The clustering of multiple failed logon events within a very short time window is consistent with potentially automated authentication attempts.

## Verdict

The observed pattern is consistent with potential brute-force or automated password-guessing activity.

The available evidence confirms multiple failed network logon attempts but does not establish whether a subsequent successful logon occurred.

## Recommended Actions

- Identify the host responsible for the authentication attempts and determine whether the activity was expected or authorized.
- Review surrounding Event ID 4625 events for additional context.
- Search for successful Event ID 4624 logons from the same source IP address after the failed attempts.
- Investigate any matching successful logons to determine whether they involve the same account and are related to the observed activity.