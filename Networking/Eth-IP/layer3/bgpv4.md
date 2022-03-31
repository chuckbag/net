# BGPv4

## Overview
(short description of BGP and when it is used)

## Discussions
1. [The Finite State Machine](bgp-the-finite-state-machine.md): Looks at how the BGP router system communicates between itself and other BGP peers. We start by looking specifically at it's state machine, and the different things it does at each state. Then we look at how it sends messages between peers (types of packets) at the different states. Then finally, we look at the basic BGP timers and how they relate to the different states.
2. [Basics Of BGP](bgp-basics-of.md): In this doc we will look at the basic steps in getting BGP up and running. This doc is slanted on working with BGP on cisco routers, so it discusses more what to do, and less on the theory. See the Appendix at the end of the doc for further reading on BGP.
3. [IP and Metric](bgp-ip-and-metrics.md): Here, we will look at how we can, or prevent, summarization of groups of IPs within BGP. We will also look at how BGP uses metrics to manipulate pathways from the end customer to te internet; with local preferences and weights (bigger is better), and from the internet (same AS#) to the end customer; with MED's and paths (smaller is better).
4. [Filters and Route Maps](bgp-filters-and-route-maps.md): Finally, we will first discuss some of BGPs special filtering abilities; globally filtering AS path info with regular expressions, filtering via routes from specific neighbors with neighbor distribution lists, and filtering routes biased on IP's with special extended access lists. Then we will look at look at how BGP distributes routes from or to BGP to other route engines with route redistributions, and then look at how bgp can group routes with very advanced search methods and even modify routes with route maps.

## Labs and Examples
- [Basic BGP Routing](bgp-routing-basic.md): This lab works with the most basic BGP setup, and showing how BGP sending routing information between routers. Note that this lab does not deal with IGP routing.

## Appendix 1. References
###  Books:
- BGP by Iljitsch Beijnum. © 2002 O'Reilly & Associates, Inc.
- Internet Routing Architectures (Second Edition) by Sam Halabi with Danny McPherson. © 2001 Cisco Press
- Cisco BGP-4 Command and Configuration Handbook by William R. PArkhurst. © 2001 Cisco Press
- IP Routing by Ravi Malhotra. © 2002 O'Reilly & Associates, Inc.
### RFCs:
- [RFC 1105](http://www.faqs.org/rfcs/rfc1105.html) [RFC 1163](http://www.faqs.org/rfcs/rfc1163.html) [RFC 1267](http://www.faqs.org/rfcs/rfc1267.html): Describes BGP prior to BGP4
- [RFC 1654](http://www.faqs.org/rfcs/rfc1654.html): Describes the first BGP specification
- [RFC 1771](http://www.faqs.org/rfcs/rfc1771.html): Describes BGP4, the current version of BGP
