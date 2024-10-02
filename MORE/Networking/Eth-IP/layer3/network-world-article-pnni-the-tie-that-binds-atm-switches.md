# Network World Article : PNNI: The tie that binds ATM switches
*This story appeared on Network World Fusion at* http://www.nwfusion.com/news/tech/0503tech.html .

## PNNI:The tie that binds ATM switches
### By MIKE WINSLOW

Network World, 05/03/99

As the need to link multivendor private and public ATM grows, so does the need to easily configure and run those environments. That's where the ATM Forum's Private Network-to-Network Interface (PNNI) comes in.

PNNI is a routing protocol for ATM that allows for the exchange of routing information between ATM switches.

As a routing protocol, PNNI allows switches in the network to determine the best route to establish a connection. In this sense, PNNI is like other protocols, such as Routing Information Protocol, but differs in that PNNI is a source-routing protocol, rather than a hop-by-hop protocol.

In hop-by-hop style routing, the link that is chosen is decided by each node. In a source-routing protocol, the first PNNI node chooses the path to be taken throughout the network.

ATM is inherently a connection oriented network and differs from datagram-style networks in that a virtual connection is established through the network before data is sent. In a datagram network, such as in IP routing, each individual data packet contains the address of the destination to which the data is being sent. Routing through the network is done on a packet-by-packet basis. In ATM, the routing decisions are made one time, when the connection is established.

Multivendor switching
PNNI allows for switches from different vendors, or on different networks, to exchange addressing and routing information so those who maintain the network can set up connections with minimum intervention. Before PNNI was accepted as a routing protocol, the standard available for the interconnection of ATM switches and networks was Interim Interface Signaling Protocol (IISP), which was a simple static route type of network.

Static routing in networks requires a relatively significant administrative effort. Defining permanent virtual circuits (PVC) in a network entails defining the link-by-link definitions that constitute the path through the network.

In order to alleviate some of the effort in PVC definition, many switch vendors implement something called switched permanent virtual circuits, which look like PVCs to the subscriber but are actually switched virtual circuits (SVC) within the network.

SVCs are created through the use of a signaling protocol. The connection is established by the network at the subscriber's demand. The subscriber specifies the endpoint address of the connection as well as the type and quality of service required by the connection.

This session is performed within the parameters of the setup message, where the required bit rate, delay and other service parameters are specified.

Address specifications
Addresses in the network can be of three types: E.164, an international telecommunications numbering standard, International Code Designator (ICD) or Data Country Code (DCC), which are ways of identifying oversees numbers. In addition, the subscriber may specify his own End Station ID (ESI) to the network.

Because the network administrator does not necessarily know the ESIs, there must be a way to communicate this information from the subscriber to the switch and propagate the address to other switches in the network.

This is accomplished at the subscriber interface through the use of Interim Link Management Interface address registration procedures. These allow the subscriber to communicate the ESI to the network and receive the network prefix from the network.

The prefix used by the node connecting to the subscriber is typically unique to the node and is known to the rest of the network. There are three distinct address formats that can be present in an ATM network. When a public and private network connect, how can they interface without redefining the address structure of the network? This problem can be addressed with the implementation of PNNI at the interface between the public and private network.

Winslow is director of products in North America for Radcom, a network equipment test vendor in Mahwah, N.J. He can be reached at Mike_ Winslow@compuserve.com.

All contents of Network World Fusion are copyright 1995-1999

by Network World, Inc., Framingham, MA 01701