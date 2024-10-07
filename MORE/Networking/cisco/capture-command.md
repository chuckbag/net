# Capture Command

## Overview
The capture command allows you to troubleshoot the asa by tracking where traffic is going, and capturing that traffic. 

## Create ACL
Capture command needs an ACL to filter on.  To capture traffic in both directions, you would need to enter two lines in the acl.  For example, if you wanted to capture all traffic between hosts 10.50.81.22 and 172.25.0.254 you would create an ACL like such:

```
fw1a# conf t
fw1a(config)# access-list cap extended permit ip host 10.50.81.22 host 172.25.0.254
fw1a(config)# access-list cap extended permit ip host 172.25.0.254 host 10.50.81.22
fw1a(config)# end
fw1a#
```

if you want to remove all the lines of the ACL and start over, simply use the clear configure command.  "no" only works for a specific line.

```
fw1a(config)# clear configure access-list cap
```

## Run Capture
Create the capture file with the following command.  note that the first "cap" is the capture name, and the second "cap" is the ACL name, "raw-data" captures all of the packet, and the capture looks only at the "inside" interface of the firewall.

```
fw1a(config)# capture cap type raw-data access-list cap interface inside
```

You can confirm that the capture is working or not with the show capture command:  (in this example, either no traffic is being sent, or a filter is blocking it. 

```
fw1a(config)# sh cap
capture cap type raw-data access-list cap interface inside [Capturing - 0 bytes]
```

If you want more details, you can look at what is hitting the ACL's:

```
fw1a(config)# sh access-list cap
access-list cap; 2 elements; name hash: 0xf034180f
access-list cap line 1 extended permit ip host 10.50.81.22 host 172.25.0.254 (hitcnt=0) 0x70ea5287
access-list cap line 2 extended permit ip host 172.25.0.254 host 10.50.81.22 (hitcnt=0) 0x894b9b9a
fw1a(config)#
```

## View Capture
Once the capture is configured, you can view what traffic has been filtered through it. 

```
fw1a# sh capture
capture cap type raw-data access-list cap interface i1-srv2 [Buffer Full - 524286 bytes]
fw1a# sh capture cap
381 packets captured
   1: 17:06:47.737846 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
   2: 17:06:47.840471 802.1Q vlan#551 P0 172.25.0.254 > 10.50.81.22: icmp: echo reply
   3: 17:06:48.922498 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
   4: 17:06:49.960491 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
   5: 17:06:50.997751 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
   6: 17:06:52.119348 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
   7: 17:06:53.382121 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
   8: 17:06:54.420037 802.1Q vlan#551 P0 10.50.81.22 > 172.25.0.254: icmp: echo request
```

## Disable Capture:
You can simply "no" out a capture command.  First review the current captures with the show command, and then no out the commands you want shut off. 

```
fw1a(config)# sh capture
capture capin type raw-data access-list cap interface outside [Capturing - 259692 bytes]
capture test type raw-data access-list test interface outside [Capturing - 472 bytes]
fw1a(config)# no cap capin
fw1a(config)# sh capture
capture test type raw-data access-list test interface outside [Capturing - 472 bytes]
fw1a(config)#
```

