# Troubleshooting VPNs

- [Troubleshooting VPNs](#troubleshooting-vpns)
  - [Check Phase1](#check-phase1)
  - [Check Phase2](#check-phase2)
  - [Confirm Working Route](#confirm-working-route)
  - [check routing](#check-routing)
  - [Capture Traffic](#capture-traffic)
  - [References](#references)



## Check Phase1
```bash
fw1b01> show security ike security-associations
Index   State  Initiator cookie  Responder cookie  Mode           Remote Address
7511026 UP     d40c69def0f2e014  783356e00cddfb70  IKEv2          96.230.36.206

fw1b01>
```

## Check Phase2
```bash
fw1b01> show security ipsec security-associations
  Total active tunnels: 1
  ID    Algorithm       SPI      Life:sec/kb  Mon lsys Port  Gateway
  <131073 ESP:3des/sha1 68e14219 1298/ unlim   U   root 500   96.230.36.206
  >131073 ESP:3des/sha1 cb3ccd42 1298/ unlim   U   root 500   96.230.36.206

fw1b01>
```

## Confirm Working Route
confirm you have a route, and it sends the traffic though the VPN: 
```bash
fw1b01> show route 10.33.32.103

inet.0: 23 destinations, 23 routes (23 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

10.33.0.0/16       *[Static/5] 01:49:14
                    > via st0.1
```

## check routing
```bash
fw01# run show route forwarding-table destination 96.20.36.204
Routing table: default.inet
Internet:
Destination        Type RtRef Next hop           Type Index    NhRef Netif
default            user     0 40:ce:24:3e:13:34  ucst     1376     3 ge-0/0/0.0
default            perm     0                    rjct       36     3

Routing table: __juniper_services__.inet
Internet:
Destination        Type RtRef Next hop           Type Index    NhRef Netif
default            perm     0                    dscd     1280     2

Routing table: __master.anon__.inet
Internet:
Destination        Type RtRef Next hop           Type Index    NhRef Netif
default            perm     0                    rjct     1305     1
```


## Capture Traffic
use traceoptions to capture traffic
```bash
fw01# show security flow traceoptions
file JTAC;
flag basic-datapath;
packet-filter MATCH1 {
    protocol icmp;
    source-prefix 10.36.36.9/32;
    destination-prefix 10.36.34.51/32;
}
packet-filter MATCH2 {
    protocol icmp;
    source-prefix 10.36.34.51/32;
    destination-prefix 10.36.36.9/32;
}
```

```bash
fw01# run show log JTAC
Feb 15 20:20:14 20:20:13.989088:CID-0:RT:<10.36.34.51/1->10.36.36.9/1;1,0x0> matched filter MATCH2:
Feb 15 20:20:14 20:20:13.989088:CID-0:RT:packet [56] ipid = 39189, @0x43e4e71c
Feb 15 20:20:14 20:20:13.989088:CID-0:RT:---- flow_process_pkt: (thd 2): flow_ctxt type 15, common flag 0x0, mbuf 0x43e4e500, rtbl_idx = 0
Feb 15 20:20:14 20:20:13.989168:CID-0:RT: flow process pak fast ifl 75 in_ifp irb.34
Feb 15 20:20:14 20:20:13.989168:CID-0:RT:  irb.34:10.36.34.51->10.36.36.9, icmp, (3/3)
Feb 15 20:20:14 20:20:13.989168:CID-0:RT: find flow: table 0x5320fb70, hash 42056(0xffff), sa 10.36.34.51, da 10.36.36.9, sp 33440, dp 39183, proto 17, tok 11, conn-tag 0x00000000
Feb 15 20:20:14 20:20:13.989168:CID-0:RT:Found: session id 0x5f5f. sess tok 11
Feb 15 20:20:14 20:20:13.989168:CID-0:RT:flow_find_session: This an Embedded ICMP pkt
Feb 15 20:20:14 20:20:13.989168:CID-0:RT:  flow got session.
```

```bash
fw01# run clear log JTAC
```

setup a filter and track packet being caught by it. 
```bash
[edit interfaces irb unit 260 family inet]
+       filter {
+           input TESTCOUNT;
+       }
[edit firewall family inet]
      filter mgmt { ... }
+     filter TESTCOUNT {
+         term 1 {
+             from {
+                 source-address {
+                     96.20.36.204/32;
+                 }
+                 destination-address {
+                     38.11.225.242/32;
+                 }
+                 protocol tcp;
+             }
+             then {
+                 count testcounter;
+                 accept;
+             }
+         }
+         term 2 {
+             then accept;
+         }
+     }
```

```bash
fwb01# run show firewall filter TESTCOUNT

Filter: TESTCOUNT
Counters:
Name                                                Bytes              Packets
testcounter                                             0                    0
```



## References
- [How do I tell if a VPN Tunnel SA (Security Association) is active?](https://kb.juniper.net/InfoCenter/index?page=content&id=kb10090&actp=METADATA): Juniper KB10090, June 2014




