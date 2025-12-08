#### GETTING TCPDUMP
---
---



##### PRECONFIGURATION
---

First we need to increase the maximum we can capture with these to settings below.
 
	pn all telcset max-packet-capture-packets 500000
	pn all telcset max-packet-capture-size 300
 
Now do the packet capture:
	
	pn 2 packet-capture --start --filter "host 10.236.8.61" any

Wait a few min. 

	pn 2 packet-capture --stop any



##### COMMAND
---

	packet-capture [--start [--snaplen ##] [--filter 'pcap filter'] <interface name>]
	[--stop <interface name>]
	[--heritage old|recent]
	Alias:       pcap
 

The packet-capture command provides the ability to capture packets from a given interface, and store the capture in a file you specify. The file containing the captured packets can then be downloaded for offline analysis with tools such as wireshark, tshark and tcpdump.
To ensure server stability, the file containing the capture is limited to a maximum of 32MB or 15,000 frames (whichever is reached first), and the file containing the captured packets should be deleted as soon as possible.
**Warning**: Server performance will be severely degraded while capturing packets, because hardware acceleration is disabled and server memory is being consumed while packets are being captured.

###### OPTIONS:
--start [--filter tcpdump-filter] [--snaplen ##] <interface>
	The --start option starts capturing on a given interface. The --snaplen option defines the amount of a packet to capture. The --filter provides the ability to filter packets on capture. The filter should conform to the syntax used for tcpdump and wireshark capture filters. The default value for --snaplen is 65535. To capture traffic from any public interface, use the special interface name "any".
--stop <interface>
	The --stop option stops capturing on a given interface and saves the capture in an ssfs file called tmp.
--heritage old|recent
	If packets need to be discarded due to resource issues, then --heritage decides which ones to keep (old or recent ones). The default is recent.



##### EXAMPLES
---

To start a packet capture on interface ag1:

	packet-capture --start ag1
 
To stop and save a capture on interface ag1:

	packet-capture --stop ag1
 
To start a packet capture on interface ag1, capturing only ICMP and ARP traffic:

	packet-capture --start --filter "icmp or arp" ag1
 
To start a packet capture on all public interfaces, capturing traffic to and from 10.1.1.83:

	packet-capture --start --filter "host 10.1.1.83" any
 
To start a packet capture on all public interfaces:

	packet-capture --start any
 
To stop a packet capture on all public interfaces:

	packet-capture --stop any
 
To retrieve the capture from server 10.1.26.64 into a local file called /tmp/asc.cap so it can be analyzed by wireshark, tshark or tcpdump:

	ssc 10.1.26.64 ssget tmp /tmp/asc.cap

To mail a capture to someone:

	nail -n -a tmp -s "icmp packet capture" myname@example.com

To remove the capture file and free up memory:

	ssrm tmp
