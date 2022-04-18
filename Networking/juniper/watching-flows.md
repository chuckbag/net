# Watching Flows
- [Watching Flows](#watching-flows)
  - [Log flows to file](#log-flows-to-file)
  - [View traffic](#view-traffic)
  - [create pcap's from the firewall](#create-pcaps-from-the-firewall)
    - [set the configs](#set-the-configs)
    - [enable](#enable)
    - [download the pcaps](#download-the-pcaps)
  - [References](#references)

## Log flows to file

clean out any old configs
```bash
cm@fw1b01# edit security flow traceoptions
cm@fw1b01# delete
Delete everything under this level? [yes,no] (no) yes
cm@fw1b01# top
```

make add the changes
```
set security flow traceoptions file JTAC-FLOW-
set security flow traceoptions flag basic-datapath
set security flow traceoptions packet-filter OUT source-prefix 96.230.36.202 destination-prefix 96.230.36.205
set security flow traceoptions packet-filter IN source-prefix 10.33.64.108 destination-prefix 96.230.36.202
```
where "JTAC-FLOW-" is just a file name where the logs will go

View the flows
```bash
cm@fw1b01# run show log JTAC-FLOW-
Apr 23 14:01:48 14:01:48.184934:CID-0:RT:CLI flow status command recvd, subtype =1
Apr 23 14:01:56 14:01:56.726032:CID-0:RT:<96.230.36.202/57426->96.230.36.205/443;6> matched filter OUT:
Apr 23 14:01:56 14:01:56.726032:CID-0:RT:packet [64] ipid = 0, @0x433d641c
Apr 23 14:01:56 14:01:56.726032:CID-0:RT:---- flow_process_pkt: (thd 2): flow_ctxt type 15, common flag 0x0, mbuf 0x433d6200, rtbl_idx = 0
Apr 23 14:01:56 14:01:56.726032:CID-0:RT: flow process pak fast ifl 76 in_ifp ge-0/0/0.0
Apr 23 14:01:56 14:01:56.726032:CID-0:RT:  ge-0/0/0.0:96.230.36.202/57426->96.230.36.205/443, tcp, flag c2 syn
Apr 23 14:01:56 14:01:56.726032:CID-0:RT: find flow: table 0x510e16a0, hash 58977(0xffff), sa 96.230.36.202, da 96.230.36.205, sp 57426, dp 443, proto 6, tok 9
Apr 23 14:01:56 14:01:56.726265:CID-0:RT:  no session found, start first path. in_tunnel - 0x0, from_cp_flag - 0
Apr 23 14:01:56 14:01:56.726265:CID-0:RT:  flow_first_create_session
Apr 23 14:01:56 14:01:56.726311:CID-0:RT:  flow_first_in_dst_nat: in <ge-0/0/0.0>, out <N/A> dst_adr 96.230.36.205, sp 57426, dp 443
Apr 23 14:01:56 14:01:56.726311:CID-0:RT:  chose interface ge-0/0/0.0 as incoming nat if.
Apr 23 14:01:56 14:01:56.726311:CID-0:RT:flow_first_rule_dst_xlate: DST xlate: 96.230.36.205(443) to 10.33.64.108(443), rule/pool id 1/1.
Apr 23 14:01:56 14:01:56.726403:CID-0:RT:flow_first_routing: vr_id 0, call flow_route_lookup(): src_ip 96.230.36.202, x_dst_ip 10.33.64.108, in ifp ge-0/0/0.0, out ifp N/A sp 57426, dp 443, ip_proto 6, tos 0
Apr 23 14:01:56 14:01:56.726420:CID-0:RT:Doing DESTINATION addr route-lookup
Apr 23 14:01:56 14:01:56.726452:CID-0:RT:  routed (x_dst_ip 10.33.64.108) from dmz1 (ge-0/0/0.0 in 0) to vlan.10, Next-hop: 10.33.64.108
Apr 23 14:01:56 14:01:56.726452:CID-0:RT:flow_first_policy_search: policy search from zone dmz1-> zone prod3 (0x110,0xe05201bb,0x1bb)
Apr 23 14:01:56 14:01:56.726510:CID-0:RT:Policy lkup: vsys 0 zone(9:dmz1) -> zone(18:prod3) scope:0
---(more)---[abort]
[edit]
cm@fw1b01#
```

## View traffic

To view the traffic between the two IPs 
```bash
cm@fw1b01> show security flow session destination-prefix 96.230.36.205 source-prefix 96.230.36.202 | refresh 2
Total sessions: 0
---(refreshed at 2018-04-23 14:09:03 UTC)---
Session ID: 320337, Policy name: igvIn/54, Timeout: 18, Valid
  In: 96.230.36.202/57454 --> 96.230.36.205/443;tcp, If: ge-0/0/0.0, Pkts: 1, Bytes: 64
  Out: 10.33.64.108/443 --> 96.230.36.202/57454;tcp, If: vlan.10, Pkts: 0, Bytes: 0
Total sessions: 1
---(refreshed at 2018-04-23 14:09:05 UTC)---
Session ID: 320337, Policy name: igvIn/54, Timeout: 16, Valid
  In: 96.230.36.202/57454 --> 96.230.36.205/443;tcp, If: ge-0/0/0.0, Pkts: 1, Bytes: 64
  Out: 10.33.64.108/443 --> 96.230.36.202/57454;tcp, If: vlan.10, Pkts: 0, Bytes: 0
Total sessions: 1
---(refreshed at 2018-04-23 14:09:07 UTC)---
Session ID: 320337, Policy name: igvIn/54, Timeout: 14, Valid
  In: 96.230.36.202/57454 --> 96.230.36.205/443;tcp, If: ge-0/0/0.0, Pkts: 1, Bytes: 64
  Out: 10.33.64.108/443 --> 96.230.36.202/57454;tcp, If: vlan.10, Pkts: 0, Bytes: 0
Total sessions: 1

---(*more 100%)---[abort]

cm@fw1b01>
```

## create pcap's from the firewall

### set the configs
Make two copies of the same config.  one to capture traffic on the outside interface, one to capture on the inside vlan: 
First copy
```
set firewall filter PCAP term 1 from source-address 96.230.36.202
set firewall filter PCAP term 1 from destination-address 96.230.36.205
set firewall filter PCAP term 1 then sample
set firewall filter PCAP term 1 then accept

set firewall filter PCAP term 2 from source-address 96.230.36.205
set firewall filter PCAP term 2 from destination-address 96.230.36.202
set firewall filter PCAP term 2 then sample
set firewall filter PCAP term 2 then accept

set firewall filter PCAP term allow-all-else then accept
```

second copy: 
```
set firewall filter PCAP-1 term 1 from source-address 96.230.36.202
set firewall filter PCAP-1 term 1 from destination-address 10.33.64.108
set firewall filter PCAP-1 term 1 then sample
set firewall filter PCAP-1 term 1 then accept

set firewall filter PCAP-1 term 2 from source-address 10.33.64.108
set firewall filter PCAP-1 term 2 from destination-address 96.230.36.202
set firewall filter PCAP-1 term 2 then sample
set firewall filter PCAP-1 term 2 then accept

set firewall filter PCAP-1 term allow-all-else then accept
```

### enable
Enable the first on the outside interface: 
```bash
cm@fw1b01# set interfaces ge-0/0/0.0 family inet filter input PCAP
cm@fw1b01# set interfaces ge-0/0/0.0 family inet filter output PCAP
```

enable the second on the inside vlan: 
```bash
cm@fw1b01# set interfaces vlan.10 family inet filter input PCAP-1
cm@fw1b01# set interfaces vlan.10 family inet filter output PCAP-1
```

### download the pcaps
confirm the data is on the firewall
```bash
cm@fw1b01# run file list /var/tmp/ detail
/var/tmp/:
total blocks: 76
-rw-r--r--  1 root  wheel       3847 Mar 8  03:59 cleanup-pkgs.log
[...]
-rw-r-----  1 root  wheel        390 Apr 23 14:22 packetcapture.ge-0.0.0
-rw-r-----  1 root  wheel        272 Apr 23 14:22 packetcapture.vlan
[...]
drwxr-xr-x  2 root  wheel        512 Oct 25 22:51 usb/
drwxrwxrwT  2 root  wheel        512 Oct 25 22:45 vi.recover/

total files: 12
```

grab the files from the firewall: 
```
mylaptop $ sftp 10.33.32.1:/var/tmp/packet* tmp/.
```

where the firewall's IP is 10.33.32.1, and I'm storing the data in my tmp dir.  


## References
- [How to create a PCAP packet capture on a J-Series or SRX branch device](https://kb.juniper.net/InfoCenter/index?page=content&id=kb11709) : Juniper Networks, Feb 2018 