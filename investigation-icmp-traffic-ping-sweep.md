Scenario:

* Unidentified Source used ICMP requests Multiple Destinations 





Evidence :

* Source IP : 192.168.1.4
* Destinations :192.168.1.3 ,192.168.1.10 , 192.168.1.255
* Sequence Number : 0 in All Requests ICMP
* ID : Changing in every request
* 192.168.1.3 , 192.168.1.255 didn't response


Analysis :

* Unidentified IP use ICMP requests to Multiple Destinations
* some Destinations have no response
* 192.168.1.3 (may firewall blocked it)
* 192.168.1.255: broadcast address, tool likely used it to sweep the network
* Sequence Number : no Incrementing \& no Continuous Ping session every session meaning : every session (request \& reply ) generated independently
* identifier (ID) : No Persistent identifier means every session taking from Unidentified source is generated independently


Verdict:

* this Pattern observed host discovery activity from the Unidentified  source

Escalation to L2 investigation and monitoring


Detection Logic:

* icmp.seq == 0 \&\& icmp.type == 8

