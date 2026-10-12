# Investigation 06 — Full Network Investigation

## 1- Executive Summary

This investigation reviewed network traffic across DNS, ICMP, TCP, HTTP, and HTTPS to identify normal and potentially suspicious activity.

The most notable findings were activity consistent with potential ICMP host discovery, TCP port scanning, and HTTP file transfer. The available evidence was insufficient to determine whether these activities were part of the same incident.

---

# Scenario

Network traffic was captured from a local lab environment and analyzed across multiple protocols, including DNS, ICMP, TCP, HTTP, and HTTPS.

The investigation focuses on identifying observed communication patterns and determining whether the activity appears normal or suspicious and requires further investigation.

---

# Network Context

The investigation was conducted within an isolated lab environment containing a Kali Linux host and a Windows host.

- Kali Linux: `192.168.1.4`
- Windows host: `192.168.1.11`

The captured traffic includes DNS, ICMP, TCP, HTTP, and HTTPS activity between lab hosts and external destinations.

The analysis focuses on identifying communication patterns and determining whether the observed activities show any meaningful relationship.

---

# DNS Activity

## Evidence

**Packet 5 — DNS Query**

- Source: `192.168.1.4:49671`
- Destination: `192.168.1.1:53`
- Query: `example.com`
- Query Type: `A`
- Transaction ID: `0xd94e`

**Packet 6 — DNS Response**

- Source: `192.168.1.1:53`
- Destination: `192.168.1.4:49671`
- Response Status: `No error`
- Transaction ID: `0xd94e`
- Returned IPv4 addresses:
  - `172.66.147.243`
  - `104.20.23.154`
- TTL: `174 seconds`

## Initial Assessment

The DNS query was sent from the Kali host to the DNS resolver at `192.168.1.1`.

The response returned successfully with `No error`, and the transaction ID matched the original query.

The response returned two IPv4 addresses for `example.com`. Multiple A records can be normal DNS behavior and are not sufficient, by themselves, to indicate suspicious activity.

The observed query-to-response interval was approximately `10.46 ms`.

---

ICMP traffic

Analysis:

- The source sent ICMP Echo Requests to multiple destinations on the network.
- The observed traffic included a request to `192.168.1.255`. The overall pattern is consistent with potential ICMP host discovery activity.

Verdict:

- The observed pattern is consistent with potential ICMP host discovery activity.

Recommended Action:

- Identify the source host and determine whether the activity was expected.

---

TCP traffic

Analysis:

- The source sent 1,053 TCP SYN packets across a range of multiple ports on the same destination.
- The destination responded with 1,037 RST, ACK packets in response to observed SYN requests.

Verdict:

- The observed pattern is consistent with potential TCP port scanning activity.

Recommended Action:

- Further investigate the source and determine whether the activity was expected.

---

HTTP traffic

Analysis:

- The source `192.168.1.11` sent an HTTP GET request for `svchost_update.ps1` to the server `192.168.1.4` on non-standard TCP port `8000`.
- The User-Agent `Microsoft-CryptoAPI/10.0` is consistent with Windows CryptoAPI activity and may indicate the use of `certutil`. Endpoint telemetry is required to confirm the responsible process.
- The requested filename contains `svchost`, a name associated with a legitimate Windows process, within a PowerShell script filename. This may indicate potential masquerading, but the filename alone does not confirm it.
- The response identified the server as `SimpleHTTP/0.6 Python/3.13.12`, indicating a lightweight Python HTTP server.

Verdict:

- The observed traffic is consistent with file-transfer activity and may represent potential Ingress Tool Transfer.

Recommended Action:

- Identify the process responsible for the download.
- Determine whether `svchost_update.ps1` was executed.
- Determine whether the transfer was expected and authorized.

---

HTTPS traffic

Analysis:

- The source generated two TLS ClientHello messages toward two destination IP addresses.
- The observed traffic showed different JA3 and JA4 fingerprints for the same SNI, `example.com`.

Verdict:

- The different JA3 and JA4 fingerprints may be consistent with the use of different client applications. No suspicious behavior was identified from the available evidence alone.

Recommended Action:

- Identify the applications responsible for the two requests.
- Determine whether the behavior was expected and authorized.

---

# Correlation / Cross-Investigation Analysis

## Scope

This investigation reviews findings from five separate network traffic investigations involving DNS, ICMP, TCP, HTTP, and HTTPS.

## Objective

The objective is to compare findings across the investigations and determine whether any meaningful relationship exists between the observed activities.

### 1- DNS Investigation

- A successful DNS resolution was observed.

### 2- ICMP Investigation

- Activity consistent with potential host discovery was observed.

### 3- TCP Investigation

- Activity consistent with potential port scanning was observed.

### 4- HTTP Investigation

- An HTTP request to retrieve a PowerShell script file was observed.

### 5- HTTPS Investigation

- TLS-encrypted traffic was observed with different JA3 and JA4 fingerprints for the same SNI.

## Correlation

### Observation 1

- The investigations identified activity consistent with potential host discovery and port scanning.
- However, no evidence was identified linking these activities to the same source or timeframe.
- Therefore, a direct correlation cannot be established based on the available evidence.

### Observation 2

- The HTTP investigation observed a request to retrieve a PowerShell file named `svchost_update.ps1`.
- No evidence was identified linking this activity to the host discovery or port scanning observations.
- Therefore, a correlation cannot be confirmed with the current evidence.

### Observation 3

- The packet captures were collected separately and do not represent a single continuous event.
- As a result, no direct correlation was established, and the available evidence does not support attributing these activities to the same incident.

---

## Overall Assessment

- The most notable findings were TCP, ICMP, and HTTP activities consistent with potential port scanning, host discovery, and file transfer, respectively.
- The DNS exchange was consistent with normal DNS resolution. The HTTPS investigation showed different JA3 and JA4 fingerprints, but this difference alone was not sufficient to identify suspicious behavior.
- The available evidence was insufficient to determine whether the observed activities were part of the same incident.

