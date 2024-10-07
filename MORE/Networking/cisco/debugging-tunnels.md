# Debugging Tunnels

- [Debugging Tunnels](#debugging-tunnels)
  - [Viewing if Phase1 Tunnels are up:](#viewing-if-phase1-tunnels-are-up)
  - [Confirming traffic over tunnels:](#confirming-traffic-over-tunnels)
  - [Confirming Interface ACLs:](#confirming-interface-acls)
  - [Confirming Route ACLs](#confirming-route-acls)
  - [References:](#references)




## Viewing if Phase1 Tunnels are up:
Once you setup a point to point tunnel, the first thing is to confirm that phase 1 (ike) session is up and working.  Use the show crypto isakmp sa command to view all the established tunnels.  The "IKE Peer" IP address is the remote IP that you are terminating the tunnel to. 

```
bos-fw1a# sh cry is sa
IKEv1 SAs:
   Active SA: 2
    Rekey SA: 0 (A tunnel will report 1 Active and 1 Rekey SA during rekey)
Total IKE SA: 2
1   IKE Peer: 214.5.14.61
    Type    : L2L             Role    : initiator
    Rekey   : no              State   : MM_ACTIVE
2   IKE Peer: 78.110.205.13
    Type    : L2L             Role    : initiator
    Rekey   : no              State   : MM_ACTIVE
There are no IKEv2 SAs
bos-fw1a#
```

If you can not see your tunnel come up, go back and make sure your phase 1 configs are correct.  The following is an example of the phase 1 configs required. 

```
crypto isakmp policy 51
 authentication pre-share
 encryption 3des
 hash sha
 group 5
 lifetime 86400

tunnel-group 184.0.116.7 type ipsec-l2l
tunnel-group 184.0.116.7 ipsec-attributes
  pre-shared-key superSecurePreSharedKey
```

To help a bit more, you can run the following debug commands to see if you can grab a bit more data from the firewall.  Once you get the logs, enter undebug all (or un all) to disable debugging. 

```
debug crypto ca
debug crypto ikev1
un all
```

You might also want to confirm that both sides are using the same pre-shared keys.   A simple sh run will hide the keys, so instead use the  more

```
bos-fw1a# sh run | be tunnel-group
tunnel-group 78.110.205.13 type ipsec-l2l
tunnel-group 78.110.205.13 ipsec-attributes
 pre-shared-key *****
bos-fw1a#
bos-fw1a# more system:running-config | be tunnel-group
tunnel-group 78.110.205.13 type ipsec-l2l
tunnel-group 78.110.205.13 ipsec-attributes
 pre-shared-key BoomChuckABowWow
```

If you make a change to the Phase1 settings, you might need to reset the session.  You can do that with the clear statement:

```
bos-fw1a# clear crypto isakmp sa  
```

## Confirming traffic over tunnels:
View traffic going over the IPSec tunnels:

```
bos-fw1a# clear crypto ipsec sa counters
bos-fw1a# sh cry ipse sa peer 24.15.14.61
peer address: 24.15.14.61
    Crypto map tag: cmap-n0-trans, seq num: 1, local addr: 68.71.10.194
      access-list b2t_traffic extended permit ip 10.50.34.0 255.255.255.0 172.16.1.0 255.255.255.0
      local ident (addr/mask/prot/port): (10.50.34.0/255.255.255.0/0/0)
      remote ident (addr/mask/prot/port): (172.16.1.0/255.255.255.0/0/0)
      current_peer: 24.15.14.61
      #pkts encaps: 72, #pkts encrypt: 72, #pkts digest: 72
      #pkts decaps: 59, #pkts decrypt: 59, #pkts verify: 59
      #pkts compressed: 0, #pkts decompressed: 0
      #pkts not compressed: 72, #pkts comp failed: 0, #pkts decomp failed: 0
      #pre-frag successes: 0, #pre-frag failures: 0, #fragments created: 0
      #PMTUs sent: 0, #PMTUs rcvd: 0, #decapsulated frgs needing reassembly: 0
      #send errors: 0, #recv errors: 0
      local crypto endpt.: 68.71.10.194/0, remote crypto endpt.: 24.15.14.61/0
      path mtu 1500, ipsec overhead 74, media mtu 1500
      current outbound spi: D6772CD4
      current inbound spi : 539AD11C
    inbound esp sas:
      spi: 0x539AD11C (1402655004)
         transform: esp-aes-256 esp-sha-hmac no compression
         in use settings ={L2L, Tunnel, }
         slot: 0, conn_id: 7634944, crypto-map: cmap-n0-trans
         sa timing: remaining key lifetime (kB/sec): (3912499/26691)
         IV size: 16 bytes
         replay detection support: Y
         Anti replay bitmap:
          0xFFFFFFFF 0xFFFFFFFF
    outbound esp sas:
      spi: 0xD6772CD4 (3598134484)
         transform: esp-aes-256 esp-sha-hmac no compression
         in use settings ={L2L, Tunnel, }
         slot: 0, conn_id: 7634944, crypto-map: cmap-n0-trans
         sa timing: remaining key lifetime (kB/sec): (3913808/26689)
         IV size: 16 bytes
         replay detection support: Y
         Anti replay bitmap:
          0x00000000 0x00000001
```

If you make a change to the phase2 configuration, you might need to reset the tunnel. To do this, use the following clear statement:

```
bos-fw1a# clear crypto isakmp sa peer 24.15.14.61
```

or depending on the version of the firewall, you might have to enter in the command this way: 

```
bos-fw1a# clear crypto ikev2 sa 24.15.14.61
```

## Confirming Interface ACLs:

You need to make sure that the traffic is not blocked by an interfaces' ACL, and that the host is allowed to send traffic to the destination's IPs. 

 
The simplest way to confirm that the ACL's are ok is with the packet-tracer command.  This just runs an example packet through the firewalls configs and tells you where they end up. This example shows traffic from interface "n2-srv1" 10.50.81.32 (tcp:12345) to 172.26.0.254 (tcp:7534).  The packet-tracer script shows that the interface ACL disallows this flow and blocks the traffic. 

```
bos-fw1a# packet-tracer input n2-srv1 tcp 10.50.81.32 12345 172.26.0.254 7534
Phase: 1
Type: ROUTE-LOOKUP
Subtype: input
Result: ALLOW
Config:
Additional Information:
in 0.0.0.0 0.0.0.0 n0-trans
Phase: 2
Type: ACCESS-LIST
Subtype:
Result: DROP
Config:
Implicit Rule
Additional Information:
Result:
input-interface: n2-srv1
input-status: up
input-line-status: up
output-interface: n0-trans
output-status: up
output-line-status: up
Action: drop
Drop-reason: (acl-drop) Flow is denied by configured rule
bos-fw1a#
```

To find out exactly what is wrong, confirm that all your rules are set right:
First, figure out what the interface name is for your source network traffic. 

```
bos-fw1a# sh ip
System IP Addresses:
Interface                Name                   IP address      Subnet mask     Method
GigabitEthernet0/0       n0-trans               68.71.10.194    255.255.255.224 CONFIG
GigabitEthernet0/1.3     purgatory              192.0.2.1       255.255.255.0   manual
GigabitEthernet0/1.100   n1-trans               10.50.128.1     255.255.255.240 CONFIG
GigabitEthernet0/1.250   n2-srv1                10.50.176.1     255.255.255.0   manual
GigabitEthernet0/1.350   n3-srv1                10.50.208.1     255.255.255.0   CONFIG
GigabitEthernet0/1.500   i1-trans               10.50.64.1      255.255.255.240 CONFIG
GigabitEthernet0/1.550   i1-srv1                10.50.80.1      255.255.255.0   CONFIG
```

Then find out what the acl group name is, which is associated to your source interface.

```
bos-fw1a(config)# sh run access-group
access-group EXTIN in interface n0-trans
access-group INTACL in interface n1-trans
access-group PCIn2 in interface n2-srv1
access-group INTACL in interface n3-srv1
access-group I1-TRANS in interface i1-trans
access-group INTACL in interface i1-srv1
access-group INTACL in interface i1-srv2
```

Then view that ACL to confirm that you are allowing traffic from the source to the destination at the other end of the VPN.  (if you are pinging, the FROM needs to be your host, and the TO in the ACL needs to be the IP address that you are trying to ping.)

```
bos-fw1a(config)# sh run access-list | be PCIn2
access-list PCIn2 remark -- traffic from the n2 pci network outward ---
access-list PCIn2 extended permit object-group JUMPABLE object-group SITENET+ object-group JUMPBOXES
access-list PCIn2 extended permit udp object-group SITENET+ object-group DNSSERVERS eq domain
access-list PCIn2 extended permit icmp object-group SITENET+ object-group SITENET+
access-list PCIn2 extended permit udp object-group SITENET+ object-group NTP-INT eq ntp
access-list PCIn2 extended permit object-group GANGLIA-PORT object-group SITENET+ object-group GANGLIA-SRV
access-list PCIn2 extended permit object-group SYSLOG_PORTS object-group SITENET+ object-group TOOL34
access-list PCIn2 extended permit tcp object-group SITENET+ object-group MX eq smtp
access-list PCIn2 extended permit tcp object-group b2oDR_VPN_FROM object-group b2oDR_VPN_TO
access-list PCIn2 extended permit icmp object-group b2oDR_VPN_FROM object-group b2oDR_VPN_TO
bos-fw1a(config)#
```

## Confirming Route ACLs
Route ACL's match to/from traffic, and put it onto a VPN tunnel.  Make sure that your traffic is properly matched, and that it will be selected to go over the VPN. 

First look at the current routes and make sure that there is no weird overlaps that might mess things up.  (are you going to mess up other traffic with this vpn?)

```
bos-fw1a# sh route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is 68.71.10.194 to network 0.0.0.0
C    68.71.10.194 255.255.255.224 is directly connected, n0-trans
C    10.50.32.0 255.255.255.0 is directly connected, oob-srv1
C    10.50.33.0 255.255.255.240 is directly connected, heartbeat
C    10.50.34.0 255.255.255.0 is directly connected, oob-trans1
C    10.50.35.0 255.255.255.0 is directly connected, oob-trans2
S    10.50.64.16 255.255.255.240 [1/0] via 10.50.64.14, i1-trans
C    10.50.80.0 255.255.255.0 is directly connected, i1-srv1
C    10.50.81.0 255.255.255.0 is directly connected, i1-srv2
C    10.50.82.0 255.255.255.0 is directly connected, i1-srv3
C    10.50.64.0 255.255.255.240 is directly connected, i1-trans
S    10.50.128.48 255.255.255.240 [1/0] via 10.50.128.8, n1-trans
```

Look at the crypto maps, and see what match address ACL is associate to the tunnel.  Also make sure that that crypto map is associate to the outside interface. 

```
bos-fw1a# sh run crypto map
[...clip...]
crypto map cmap-n0-trans 7 match address b2oDR_traffic
crypto map cmap-n0-trans 7 set peer 24.15.14.61
crypto map cmap-n0-trans 7 set transform-set TRANS_ESP_3DES_SHA
crypto map cmap-n0-trans 7 set pfs group5
crypto map cmap-n0-trans interface n0-trans
```

Then view the specific match address ACL, and ensure that it captures the flows between the sources and destination.  (Remember, if you want pings to go through, you need to call it out!)

```
bos-fw1a# sh run access-list | in b2oDR_traffic
access-list b2oDR_traffic extended permit tcp object-group b2oDR_VPN_FROM object-group b2oDR_VPN_TO
access-list b2oDR_traffic extended permit icmp object-group b2oDR_VPN_FROM object-group b2oDR_VPN_TO
```

Track down all the object groups as needed

```
bos-fw1a# sh run object-group id b2oDR_VPN_TO
object-group network b2oDR_VPN_TO
 description --- systems we can connect to ---
 network-object 172.26.0.0 255.255.255.0
```

Make sure that there are double-nat's setup to allow the traffic from your interface out:

```
bos-fw1a# sh run nat | in b2o
nat (n2-srv1,n0-trans) source static b2oDR_VPN_FROM b2oDR_VPN_FROM destination static b2oDR_VPN_TO b2oDR_VPN_TO
bos-fw1a#
```

More

```
debug icmp trace 255
un all
capture capin
    create acl to filter traffic:
    access-list {name} int {source}
sh cap capin
```

## References:
- [Cisco ASA 5500 Series Adaptive Security Appliances](http://www.cisco.com/en/US/products/ps6120/products_tech_note09186a00807e0aca.shtml): Most Common L2L and Remote Access IPsec VPN Troubleshooting Solutions  