# DNS Query and Response Investigation

## Scenario

Host `192.168.1.4` sent a DNS query for `example.com` to the DNS resolver `192.168.1.1`. The resolver returned a response containing IPv4 addresses for the requested domain.

---

## Evidence

### Packet 5 — DNS Query

**Ethernet (Data Link Layer)**
- Source MAC: `08:00:27:8a:35:d2`
- Destination MAC: `78:5c:5e:6c:0b:62`

**IPv4 (Network Layer)**
- Source IP: `192.168.1.4`
- Destination IP: `192.168.1.1`

**UDP (Transport Layer)**
- Source Port: `49671`
- Destination Port: `53`

**DNS (Application Layer)**
- Transaction ID: `0xd94e`
- Query Name: `example.com`
- Query Type: `A`
- Response In: `Packet 6`

### Packet 6 — DNS Response

**Ethernet (Data Link Layer)**
- Source MAC: `78:5c:5e:6c:0b:62`
- Destination MAC: `08:00:27:8a:35:d2`

**IPv4 (Network Layer)**
- Source IP: `192.168.1.1`
- Destination IP: `192.168.1.4`

**UDP (Transport Layer)**
- Source Port: `53`
- Destination Port: `49671`

**DNS (Application Layer)**
- Response Status: `No error`
- Transaction ID: `0xd94e`
- Returned IPv4 addresses:
  - `172.66.147.243`
  - `104.20.23.154`
- TTL: `174 seconds`
- UDP Payload Size: `1232 bytes`

### DNS Query Correlation

The query and response share the same transaction ID, `0xd94e`, and the response is linked to Packet 5 through Wireshark's `Response In` field.

The query was captured at `44.168797193` seconds, and the response at `44.179256106` seconds, giving an observed query-to-response interval of approximately `10.46 ms`.

---

## Analysis

- Host `192.168.1.4` sent a DNS query to the resolver at `192.168.1.1` using UDP port `53`.
- The resolver returned a successful response with the matching transaction ID.
- The response contained two IPv4 A records for `example.com`. Multiple A records can be normal DNS behavior and do not, by themselves, indicate suspicious activity.
- The observed query-to-response interval was approximately `10.46 ms`.
- No suspicious behavior was identified in this DNS exchange based on the available evidence.

---

## Verdict

**No suspicious activity identified in the captured DNS exchange.**

The observed traffic is consistent with a normal DNS query and response:
- Expected query and response direction.
- Matching DNS transaction ID.
- Successful response status.
- Two IPv4 addresses returned for the requested domain.
- No suspicious behavior identified from the available evidence.

---

## Recommended Action

No escalation is required based on this DNS exchange alone. Further investigation would be warranted if other evidence indicated suspicious activity involving the host or the resolved addresses.
