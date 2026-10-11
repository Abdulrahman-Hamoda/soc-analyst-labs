#Scenario :
- Two TLS client Hello packets were observed for the same SNI different JA3 fingerprint 
----------

#Evidence :
* packet 12 
- Source = 192.168.1.4
- Destination = 104.20.23.154 
- SNI = example.com
- ALPN Next Protocol: http/1.1
- [JA3: 6f7889b9fb1a62a9577e685c1fcfa919]
- supported versions (Len=5) TLS 1.3, TLS 1.2

* packet 593 
- Source = 192.168.1.4
- Destination = 172.66.147.243
- SNI = example.com 
- ALPN Next Protocol: http/1.1
- [JA3: 1d573f07cf9592c93700cd3f524279e0]
- supported versions (Len=5) TLS 1.3, TLS 1.2

----------
#Analysis :
- the source generated two requests client hello on tls connection toward two Destination IPs, pattern showing a different JA3 

- the traffic shows different JA3 fingerprint for the same SNI 

- This indicates two different TLS client libraries generated the connections - consistent with different applications (e.g., a browser vs a command-line tool) 

- Further investigation should identify the applications generating connections if the behavior is unexpected 

--------
#Verdict :
- The observed traffic shows different JA3 fingerprint for the same SNI 








