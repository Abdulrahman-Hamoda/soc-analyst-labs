#Scenario: 
- a source IP device 192.168.1.11 sent request http to server 192.168.1.4 for svchost_update.ps1 file.  

----------------------

#Evidence 

request: 
- Source IP : 192.168.1.11
- Destination : 192.168.1.4 
- Source port : 49743
- Destination port : 8000
- Request URI: /svchost_update.ps1
- User-Agent: Microsoft-CryptoAPI/10.0\r\n
- Host: 192.168.1.4:8000\r\n
- Pragma: no-cache\r\n
----
Response : 
- Source IP : 192.168.1.4
- Destination : 192.168.1.11 
- source port :8000
- destination : 49743
- Response Version: HTTP/1.0
- Server: SimpleHTTP/0.6 Python/3.13.12\r\n
- Content-type: application/octet-stream\r\n

-------------------
#Analysis :
- The Source 192.168.1.11 sent request  HTTP GET file svchost_update.ps1 on identified Server 192.168.1.4 on Non-standard port 8000 . 
- This HTTP traffic was observed on TCP/8000 , a Non-standard port for HTTP compared with the conventional http TCP/80.
- The User-agent is consistent with Windows CryptoAPI activity and may indicate the use of  certutil ; endpoint telemetry is required to confirm the responsible process.
- The response identifies the server is a SimpleHTTP/0.6 Python indicating a lightweight python HTTP server.
- The filename is Suspicious because it uses a trusted windows process name "svchost" in a PowerShell script filename , which may indicate masquerading.

-----------------
#Verdict :
- The observed traffic is Suspicious and consistent with an ingress tool transfer Scenario.
- Further endpoint investigation endpoint is required to identify the process responsible for the download and determine whether the file was executed. 

MITRE ATT&CK:

T1105 — Ingress Tool Transfer

T1036 — Masquerading may apply due to the suspicious filename, but this behavior is not confirmed.




