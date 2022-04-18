# Configuring / Troubleshooting BGP

## Setting up the link: 

Define the interface for the bgp link: 
```
interfaces {
    ge-0/0/2 {
        description "Frankfurt BGP link";
        speed 100m;
        link-mode full-duplex;
        unit 0 {
            family inet {
                address 5.72.7.26/30;
            }
        }
    }
}
```

make sure you advertise your entire /24, even if some routes are unknown by the router (aggregate).  Also define the IP of the router, and your AS: 
```
routing-options {
    aggregate {
        route 19.23.23.0/24;
    }
    router-id 10.120.0.253;
    autonomous-system 4467;
}
```

Define the bgp filter that will advertise our route, but block everyone else's: 
```
policy-options {
    policy-statement as4910-export {
        term allowOurs {
            from {
                protocol aggregate;
                route-filter 19.23.23.0/24 exact;
            }
            then accept;
        }
        term blockOthers {
            then reject;
        }
    }
}
```

enable bgp, and apply the route filter to the outbound flow
```
protocols {
    bgp {
        group fra-carrier-AS4910 {
            type external;
            description "the email, phone #, and ckt id from the carrier";
            export as4910-export;
            peer-as 4910;
            neighbor 5.72.7.25;
        }
    }
}
```

## Confirming we're getting the routes, and sending out the routes: 
Make sure the link is up, and that we are established!  
```bash
me@fra-rt1b02> show bgp neighbor 5.72.7.25
Peer: 5.72.7.25+25596 AS 4910 Local: 5.72.7.26+179 AS 4467
  Description: the email, phone #, and ckt id from the carrier
  Type: External    State: Established    Flags: <Sync>
  Last State: OpenConfirm   Last Event: RecvKeepAlive
  Last Error: Hold Timer Expired Error
  Export: [ as4910-export ]
  Options: <Preference PeerAS Refresh>
  Holdtime: 90 Preference: 170
  Number of flaps: 348
  Last flap event: RecvNotify
  Error: 'Open Message Error' Sent: 0 Recv: 34
  Error: 'Update Message Error' Sent: 0 Recv: 1
  Error: 'Hold Timer Expired Error' Sent: 4 Recv: 1
  Error: 'Cease' Sent: 2 Recv: 48
  Peer ID: 5.72.7.29   Local ID: 10.120.0.253      Active Holdtime: 90
  Keepalive Interval: 30         Peer index: 0
  BFD: disabled, down
  Local Interface: ge-0/0/2.0
  NLRI for restart configured on peer: inet-unicast
  NLRI advertised by peer: inet-unicast
  NLRI for this session: inet-unicast
  Peer supports Refresh capability (2)
  Stale routes from peer are kept for: 300
  Peer does not support Restarter functionality
  Peer does not support Receiver functionality
  Peer supports 4 byte AS extension (peer-as 48910)
  Peer does not support Addpath
  Table inet.0 Bit: 10000
    RIB State: BGP restart is complete
    Send state: in sync
    Active prefixes:              106374
    Received prefixes:            106374
    Accepted prefixes:            106374
    Suppressed due to damping:    0
    Advertised prefixes:          0
  Last traffic (seconds): Received 0    Sent 17   Checked 17
  Input messages:  Total 20806  Updates 20805   Refreshes 0     Octets 1664188
  Output messages: Total 3      Updates 0       Refreshes 0     Octets 97
  Output Queue[0]: 0
  Received and buffered octets: 22
```

check to see that we are sending out the right routes: 
```bash
me@fra-rt1b02> show route advertising-protocol bgp 5.72.7.25

inet.0: 532747 destinations, 1062299 routes (532747 active, 0 holddown, 0 hidden)
  Prefix                  Nexthop              MED     Lclpref    AS path
* 19.23.23.0/24        Self                                    [4467] I
```

also check that we are getting routes from our peer: 
```bash
me@fra-rt1b02> show route receive-protocol bgp 5.72.7.25

inet.0: 532748 destinations, 1062295 routes (532748 active, 0 holddown, 0 hidden)
  Prefix                  Nexthop              MED     Lclpref    AS path
* 1.0.0.0/24              5.72.7.25                           4910 174 15169 I
* 1.0.4.0/24              5.72.7.25                           4910 174 4826 56203 I
* 1.0.5.0/24              5.72.7.25                           4910 174 4826 56203 I
* 1.0.6.0/24              5.72.7.25                           4910 174 4826 56203 I
* 1.0.7.0/24              5.72.7.25                           4910 174 4826 56203 56203 56203 I
* 1.0.64.0/18             5.72.7.25                           4910 1299 3356 2516 7670 18144 I
* 1.0.128.0/17            5.72.7.25                           4910 174 38040 9737 I
```

Get a nice summary of the entire bgp status: 
```bash
me@fra-rt1b02> show bgp summary
Groups: 1 Peers: 1 Down peers: 0
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0           1062239     532722          0          0          0          0
Peer                AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
5.72.7.25         4910     136381         70       0     348       29:43 529699/529699/529699/0 0/0/0/0
```

review the details of your routes, and if you have multiple peers, each path for each prefix(route)
```bash
me@fra-rt1b02> show route protocol bgp

inet.0: 532740 destinations, 1062273 routes (532740 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

1.0.0.0/24         *[BGP/170] 00:31:56, localpref 100
                      AS path: 4910 174 15169 I
                    > to 5.72.7.25 via ge-0/0/2.0
1.0.4.0/24         *[BGP/170] 00:32:09, localpref 100
                      AS path: 4910 174 4826 56203 I
                    > to 5.72.7.25 via ge-0/0/2.0
```



## References: 
- [Junos for dummies cheat sheet](http://www.dummies.com/how-to/content/junos-os-for-dummies-cheat-sheet.html): 