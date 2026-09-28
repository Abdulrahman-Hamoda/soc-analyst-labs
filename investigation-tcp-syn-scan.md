\#Scenario :
unidentified source sent 1053 SYN requests on protocol tcp to 1 - 50000+ ports on destination



\#Evidence :

* Source = 192.168.1.4
* Destination = 45.33.32.156
* Source port = 54651
* Destination port = 1 - 50000+
* SYN Packets observed = 1053
* RST,ACK packets observed = 1037
* Flags = SYN and RST,ACK
* window = 1024



\#Analysis :

* The Source sent 1053 TCP SYN requests to 50000+ destination ports.
* The destination responded with 1037 RST,ACK packets to the observed SYN requests
* Unlike a normal TCP Connection , the observed traffic did not complete the TCP three-way handshake :
SYN → SYN/ACK → ACK
Instead the observed pattern was
SYN → RST, ACK
* Further investigation should identify the source host and determine who or what generated the traffic.



\#Verdict :

* The observed traffic is consistent with TCP SYN -based port/service discovery (MITRE ATT\&CK T1046, Network Service Discovery).
* next further should focus on knowledge source ip
* Continue monitoring the source for additional scanning activity

