# Ethernet and IP Stacks

## Understanding Ethernet & IP

### Ethernet Theory
- [Ethernet Analyzed](layer2/EthernetAnalyzed.md). A detailed look at the lower level ethernet frame.
- [Ethernet and IP in a nutshell](layer2/Ethernet3-1.pdf). A slide show (ppt) overview on Layers 2-4.

### Layer 2 Protocols
- [Spanning Tree Protocol](layer2/SpanningTreeProtocol.md).  A review of the dreaded spanning tree, how it works, and the different versions
- VLAN Trunking Protocols: Discusses how VLAN management is supported and implemented.

### IP Theory
- [Understanding IP Addressing: Everything You Ever Wanted To Know - By Chuck Semeria](https://cse.buffalo.edu/~hungngo/classes/2010/589/reading-materials/IP-addressing.pdf). This is actually a great guide! So good that I downloaded the [pdf's](layer3/IP-addressing.pdf) for backup incase I couldn't get to the link anymore.
- TCP Performance-Tuning. This is a good doc explaining how the IP sliding windows work, and how to work with them.
- Solving ip masking problems. I want to create a page that helps people understand how ip's and masks work together. A kind of "how-to" on CIDR. Anyway this is where that page will be.
- [Interactive IP Stack](http://www.cs.columbia.edu/~hgs/internet/). Shows the osi model and hot links describing what they are and more links and info on them.

### Layer4 Theory
- IP/TCP/UDP/ICMP Poster. A nice poster to printout on a plotter. Shows all of the IP Stack's packets and some of their flow diagrams.
- [IPSec Theory](layer3/ipsec-theroy.md): AH/ESP
  
## Managing IP Space
- [Getting IP Addresses](layer3/GettingIPaddresses.md): How to request IP's and BGP ASN's from the registries
- [RFC1918](http://www.faqs.org/rfcs/rfc1918.html): Address Allocation for Private Internets. 
- [RFC 1878](http://www.faqs.org/rfcs/rfc1878.html): Variable Length Subnet Table For IPv4
- [RFC 3330](http://www.faqs.org/rfcs/rfc3330.html): Special-Use IPv4 Addresses. ie: 10.0.0.0/8, 0.0.0.0/8, and 169.254.0.0/16 to name a few.
- [RFC 1466](http://www.faqs.org/rfcs/rfc1466.html): Guidelines for Management of IP Address Space, and a successor, RFC 2101: IPv4 Address Behavior Today.
- [Break out of ip blocks](layer3/ipv4-breakout.md) in [text](layer3/subnet-blocks.txt), [pdf](layer3/subnet-blocks.pdf) and [visio](layer3/subnet-blocks.vsd). This shows the blocks of IP's and how they can be masked together. (visually, how would you group a bunch of /30's with a bunch of /29's with a couple /28's, /27's, /26's and one /25.)
- [Choosing private IP space for your business](layer3/choosing-private-ip-space-for-your-business.md): A plan on how to provide IPs to different offices, and how to break it out within the office.  

## Ethernet/IP Configuration on other Hosts
- Linux IP Settings. A brief reminder on how all the Ethernet and IP stacks come together on the Linux/Redhat platform.

## Analyzing and Monitoring Network Traffic
This section is kind of a bridge between the "analyzing ethernet and ip" section, and the "security" section. Here we will be looking at ways to view traffic, debug problems, and use different tools to help analyse what is going on.

### Procedures for testing:
- Spaning and tcpdump. This doc explains how to setup cisco switches to "span" or to mirror traffic between two hosts to another sniffing host. It also describes some basic unix tcpdump commands to sniff the traffic and to save it to a file to be later analysed with ethereal.
- Traffic and Bandwidth Testing Procedures. A simple set of steps that can be used to test different networking equipment to see how they behave when traffic is sent through them.

### Tools for testing:
- [ethereal](http://www.ethereal.com/). A tool for viewing tcpdump (and other) files. This tool lets you view all the individual packets, and explains the packets in detail.
- [tcptrace](http://www.tcptrace.org/). A tool for analyzing traffic flows from tcpdump (and other) files. This tool lets you analyse traffic statistics, allowing you to see tcp problems with traffic flows.
- [iperf](http://dast.nlanr.net/Projects/Iperf/) network bandwidth testing utility, and [tcpplot](http://www.internet2.edu/~shalunov/tcpplot/) a TCP throughput dashboard.
- [ftrace](http://www.brendangregg.com/blog/2014-09-06/linux-ftrace-tcp-retransmit-tracing.html): better views of packet issues from the side of the linux host. 
- [Tools for modeling the user-traffic](http://www.comlab.uni-rostock.de/research/tools.html) "The following list outlines tools for the collection of user-generated traffic, the analysis of thus data and related software i.e. for manipulating the recorded data."
- [Traffic Generators](http://www.fokus.fhg.de/research/cc/glone/projects/ip-qos/tools/traffic_gen.html). "Tools for generating artificial traffic mimicking real traffic for measurement, testing"

## Appendix A- References
- [RFC1700: Assigned Numbers](http://www.faqs.org/rfcs/rfc1700.html) Also in Local PDF Format. This is the motherland of references for ip numbers. It specifies all ip types, TCP and UDP reserved ports and a whole lot more! But it is outdated and replaced by [RFC 3232](http://www.faqs.org/rfcs/rfc3232.html): rfc 1700 is Replaced by an On-line Database
- [IEEE's OUI Site Search](http://standards.ieee.org/regauth/oui/index.html). Have you ever looked at a MAC address, and wondered who it belonged to? This page will search the OID (first 6 hex digits) and tell you who made the NIC.
- [RFC 2825](http://www.faqs.org/rfcs/rfc2825.html). A Tangled Web: Issues of I18N, Domain Names, and the Other Internet protocols
  