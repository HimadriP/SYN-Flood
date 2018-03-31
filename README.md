# SYN-Flood
Implementation of a basic TCP Syn Flood Attack and its counter measure \[on linux systems\].

## Attack
The source address is spoofed and TCP SYN requests are sent over the network to the target host. 

## Counter Measure
The approach applied to counter this is mentioned in the _ProcessPacket_ function of `sniffer.c`.
* A linked list is maintained where each element is associated with an ip address and a count of the number of SYN requests sent. 
* The incoming SYN requests are matched with the ip addresses and their counts are incremented. 
* When the number of SYN requests from a particular ip address increases 4096 (value taken as default) that ip is considered as an attacker and is removed using the `ip tables` command.

## Improvements (which could be introduced in the counter measure)
* The linked list needs to be refreshed at regular intervals for ips which haven't sent a request in a _while_. ( A timeout for ips needs to be introduced to refresh their counts )
* There is no cap on the size of the linked list. A maximum limit could be introduced.
* The Search for the ip is a straightforward Linear Search. This could be imporved with a hash map.
* The Limit on the amount of packets sent to be classified as an attacker could be calculated dynamically and in a more intelligent fashion where the network is observed and the limit is calculated based on incoming traffic and congestion.

This approach is very straightforward and would not work in a practical environment. This aim of this project was to learn the structure of TCP packets and the header formats and also to try and spoof a basic SYN attack. 

## Notes
For more information regarding SYN Floods refer this [article](https://www.cisco.com/c/en/us/about/press/internet-protocol-journal/back-issues/table-contents-34/syn-flooding-attacks.html).
