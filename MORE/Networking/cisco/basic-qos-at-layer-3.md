# Basic QoS at Layer 3

When setting up a basic CB marking, we need to do the following steps:
1. We first need to define our access lists (lines 28-32) to find the traffic that we want to classify.
2. Next, we need to create a class map that links to an access list (lines 4-7).  You could think of this step as naming the access list.
3. Now we create the policy map with whatever class maps should be included (lines 10-17), and define how we should mark the traffic
4. Finally, we need to bind a policy map to an interface (line 26).  Note that you can bind two policy maps to an interface, one inbound, and one out.

## Example of Basic CB Marking

```
!
ip cef
!
class-map match-all HI
   match access-group 101
class-map match-all LO
   match access-group 100
!
!
policy-map IPFRCOS
   class HI
      set ip dscp af31
      bandwidth percent 35
   class LO
      set ip dscp af21
   class class-default
      set ip dscp default
!
!
interface Serial0/0/0
   description description IPFR AT&T Ckt ID:
   bandwidth 1536
   ip address 10.109.2.81 255.255.255.252
   encapsulation frame-relay IETF
   frame-relay lmi-type cisco
   service-policy output IPFRCOS
!
access-list 101 permit tcp any eq telnet any
access-list 101 permit tcp any any eq telnet
access-list 101 permit icmp any any
access-list 101 permit tcp any any eq 1494
access-list 101 permit udp any any eq 1604
!
```

## Cisco's Recommended Values for Marking:

IETF RFCs have defined special keywords, called Per-Hop Behaviors, for specific DSCP markings

- EF: Expedited Forwarding (RFC3246)  
  - (DSCP 46)
- CSx: Class Selector (RFC2474)
  - Where x corresponds to the IP Precedence value (1–7)
  - (DSCP 8, 16, 24, 32, 40, 48, 56)
- AFxy: Assured Forwarding (RFC2597)
  - Where x corresponds to the IP Precedence value (only 1–4 are used for AF Classes)
  - And y corresponds to the Drop Preference value (either 1 or 2 or 3)
  - With the higher values denoting higher likelihood of dropping
  - (DSCP 10/12/14, 18/20/22, 26/28/30, 34/36/38)
- BE: Best Effort or Default Marking Value (RFC2474)
  - (DSCP 0)


Type of Service                     | CoS | Precedence   | DSCP
------------------------------------|-----|--------------|------
Voice Payload                       | 5   | 5            | EF
Video Payload	                     | 4	| 4	         | AF41
Voice/Video Signaling	            | 3   | 3            | AF31
High-priority or gold data classes	| 2	| 2	         | AF2x (x=1-3)
Medium-priority or silver data	   | 1	| 1         	| AF1x (x=1-3)
All else	                           | 0	| 0         	| Default