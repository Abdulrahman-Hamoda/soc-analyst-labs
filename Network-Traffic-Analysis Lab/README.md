\# Network Traffic Analysis Lab



Practical SOC Analyst L1 investigations analyzing network traffic patterns using Wireshark across multiple protocols.



\## Investigations



| # | Protocol | Investigation |

|---|---|---|

| 01 | DNS | DNS Query and Response Analysis |

| 02 | ICMP | Host Discovery Sweep — T1018 |

| 03 | TCP | SYN Port Scan — T1046 |

| 04 | HTTPS/TLS | JA3 Fingerprint Analysis — T1071.001 |

| 05 | HTTP | Suspicious File Transfer — T1105, potential T1036 |

| 06 | Multi-Protocol | Full Network Investigation and Cross-Investigation Analysis |



\## Investigation Overview



\### 01 — DNS Query and Response Analysis

Analyzed a DNS query for `example.com` and its corresponding response. Verified the matching transaction ID, successful response status, and returned IPv4 addresses.



\### 02 — ICMP Host Discovery

Analyzed ICMP Echo Requests sent to multiple destinations to assess whether the observed pattern was consistent with potential host discovery.



\### 03 — TCP SYN Port Scan

Examined TCP SYN requests and RST, ACK responses to identify a pattern consistent with potential port scanning.



\### 04 — HTTPS/TLS Fingerprint Analysis

Compared JA3 and JA4 fingerprints from TLS ClientHello traffic associated with the same SNI, `example.com`.



\### 05 — Suspicious HTTP File Transfer

Investigated an HTTP GET request for `svchost\_update.ps1` over TCP port 8000. The observed traffic was consistent with a potential file-transfer activity requiring endpoint investigation.



\### 06 — Full Network Investigation

Reviewed findings across five separate DNS, ICMP, TCP, HTTP, and HTTPS investigations. Evaluated possible relationships while accounting for the fact that the packet captures were collected separately.



\## Tools and Environment



\- Wireshark

\- Nmap

\- Kali Linux

\- Windows 10

\- VirtualBox



\## Investigation Methodology



Each investigation focuses on:



\- Capturing and examining network evidence

\- Identifying relevant communication patterns

\- Comparing observations with expected behavior

\- Assessing suspicious activity without overstating conclusions

\- Documenting findings, verdicts, and recommended actions



\## Scope and Limitations



These are practical lab investigations. Suspicious patterns indicate areas for further investigation and do not independently prove malicious intent.



The full network investigation compares separate captures; it does not establish that all observed activities belong to one incident.





