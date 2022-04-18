# Debugging Netscreen

## Overview:
The following goes over the process for debugging a flow in a netscreen firewall

## Finding policy allowing a specific flow:

Clear the active log
```
mfw01a.atl(M)-> clear db
```

Remove any current filters
```
mfw01a.atl(M)-> unset ff
filter 0 removed
```

Create a filter between the source (src-ip) and the destination (dst-ip)
```
mfw01a.atl(M)-> set ff src-ip 172.19.166.24 dst-ip 10.218.23.130
filter added
```

Turn on the filtering
```
mfw01a.atl(M)-> debug flow basic
```

See the results. 

Note this will show you (among other things)
- the inbound interfaces (eth1/1)
- the outbound interface (eth1/2.1)
- the outbound route (route 10.193.23.130->10.218.0.17, to ethernet1/2.1)
- The policy allowing this flow (919)

```
mfw01a.atl(M)->
mfw01a.atl(M)-> get db stream
**st: <mgmt-hsbb|ethernet1/1|Root|0> 4815c40: 8870:172.16.42.136/6296->10.193.23.130/50,6,60
****** 7301845.0: <mgmt-hsbb/ethernet1/1> packet received [60]******
  ipid = 34928(8870), @04815c40
  packet passed sanity check.
  flow_decap_vector IPv4 process
  ethernet1/1:172.16.42.136/25238->10.193.23.130/80,6<Root>
  no session found
  flow_first_sanity_check: in <ethernet1/1>, out <N/A>
  chose interface ethernet1/1 as incoming nat if.
  flow_first_routing: in <ethernet1/1>, out <N/A>
  search route to (ethernet1/1, 172.16.42.136->10.193.23.130) in vr trust-vr for vsd-0/flag-0/ifp-null
  [ Dest] 334.route 10.193.23.130->10.218.0.17, to ethernet1/2.1
  routed (x_dst_ip 10.193.23.130) from ethernet1/1 (ethernet1/1 in 0) to ethernet1/2.1
  policy search from zone 1005-> zone 1004
 policy_flow_search  policy search nat_crt from zone 1005-> zone 1004
  RPC Mapping Table search returned 0 matched service(s) for (vsys Root, ip 10.193.23.130, port 80, proto 6)
  No SW RPC rule match, search HW rule
swrs_search_ip: policy matched id/idx/action = 919/301/0x9
  Permitted by policy 919
  No src xlate   choose interface ethernet1/2.1 as outgoing phy if
  check nsrp pak fwd: in_tun=0xffffffff, VSD 0 for out ifp ethernet1/2.1
  vsd 0 is active
  no loop on ifp ethernet1/2.1.
  session application type 6, name HTTP, nas_id 0, timeout 300sec
  service lookup identified service 0.
  flow_first_final_check: in <ethernet1/1>, out <ethernet1/2.1>
  existing vector list 122-efc04f4.
  Session (id:523898) created for first pak 122
  flow_first_install_session======>
  route to 10.218.0.17
  arp entry found for 10.218.0.17
  ifp2 ethernet1/2.1, out_ifp ethernet1/2.1, flag 00800004, tunnel ffffffff, rc 1
  outgoing wing prepared, ready
  handle cleartext reverse route
  search route to (ethernet1/2.1, 10.193.23.130->172.16.42.136) in vr trust-vr for vsd-0/flag-3000/ifp-ethernet1/1
  [ Dest] 227.route 172.16.42.136->10.218.72.4, to ethernet1/1
  route to 10.218.72.4
  arp entry found for 10.218.72.4
  ifp2 ethernet1/1, out_ifp ethernet1/1, flag 00800001, tunnel ffffffff, rc 1
Success installing work and forward sessions
  flow got session.
  flow session id 523898
  flow_main_body_vector in ifp ethernet1/1 out ifp ethernet1/2.1
  flow vector index 0x122, vector addr 0xefc04f4, orig vector 0xefc04f4
  vsd 0 is active
  tcp seq check.
  transfer packet to hardware.
**st: <mgmt-hsbb|ethernet1/1|Root|0> 482afc0: 8d34:172.16.42.136/629e->10.193.23.130/50,6,60
****** 7301846.0: <mgmt-hsbb/ethernet1/1> packet received [60]******
  ipid = 36148(8d34), @0482afc0
```


Confirming the route, zone and interface for a specific IP:
```
mfw01a.atl(M)-> get route ip 211.151.53.232
 Dest for 211.151.53.232
--------------------------------------------------------------------------------------
trust-vr       : => 0.0.0.0/0 (id=26) via 10.196.1.33 (vr: trust-vr)
                    Interface ethernet1/1.3 , metric 1
mfw01a.atl(M)->
```

Confirming a specific interface:
```
mfw01a.atl(M)-> get interface ethernet1/1.3
Interface ethernet1/1.3(VSI):
  description ethernet1/1.3
  number 7, if_info 229344, if_index 3, VLAN tag 1055, mode route
  link up, phy-link up/full-duplex, admin status up
  vsys Root, zone service, vr trust-vr, vsd 0
  dhcp client disabled
  *ip 10.196.1.34/29   mac 0010.dbff.2070
  *manage ip 10.196.1.34, mac 0022.83ac.a807
  route-deny disable
  pmtu-v4 disabled
  ping enabled, telnet disabled, SSH disabled, SNMP disabled
  web disabled, ident-reset disabled, SSL disabled
  DNS Proxy disabled, webauth disabled, g-arp enabled, webauth-ip 0.0.0.0
  RIP disabled  RIPng disabled
  NSGP disabled  mtrace disabled
  PIM: not configured  IGMP not configured
  MLD not configured
  NHRP disabled
  bandwidth: physical 0Mbps, configured 0Mbps
  DHCP-Relay disabled at interface level
  DHCP-server disabled
mfw01a.atl(M)->
```

