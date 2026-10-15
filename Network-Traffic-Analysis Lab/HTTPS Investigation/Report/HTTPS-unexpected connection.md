# Scenario
- Two TLS ClientHello packets were observed from the same source IP, targeting two different destination IPs with the same SNI but different JA3 fingerprints.

---

# Evidence

**Packet 12**
- Source: `192.168.1.4`
- Destination: `104.20.23.154`
- SNI: `example.com`
- ALPN: `http/1.1`
- JA3: `6f7889b9fb1a62a9577e685c1fcfa919`
- Supported versions: TLS 1.3, TLS 1.2

**Packet 593**
- Source: `192.168.1.4`
- Destination: `172.66.147.243`
- SNI: `example.com`
- ALPN: `http/1.1`
- JA3: `1d573f07cf9592c93700cd3f524279e0`
- Supported versions: TLS 1.3, TLS 1.2

---

# Analysis
- The same source IP initiated TLS connections toward two different destination IPs.
- Both ClientHello packets contained the same SNI, `example.com`, but different JA3 fingerprints.
- The fingerprint difference indicates differences in the TLS ClientHello characteristics used to calculate JA3. It does not, by itself, prove that different applications or TLS libraries generated the connections.
- If the difference is unexpected, further investigation could correlate the connections with process-level telemetry and compare the detailed ClientHello fields.

---

# Verdict
- Two different JA3 fingerprints were observed for TLS ClientHello packets sharing the same SNI and source IP but targeting different destination IPs.
- The available packet evidence alone is insufficient to determine whether the difference reflects separate applications, different client configurations, or another cause.




