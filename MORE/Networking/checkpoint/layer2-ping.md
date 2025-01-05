# layer2 ping
Ping at layer 2, avoiding any ACL issues:  (but since it's layer 2, it only pings devices on the local segments.) 
```
[Expert@BOS-CKP-FW1:0]# arping -I eth5.999 10.153.99.27

ARPING 10.153.99.27 from 10.153.99.2 eth5.999
Unicast reply from 10.153.99.27 [00:50:56:A9:33:2A]  1.490ms
[...]
Unicast reply from 10.153.99.27 [00:50:56:A9:33:2A]  1.423ms
Sent 88 probes (64 broadcast(s))
Received 25 response(s)
```

Check what you already have in the arp cache
```
[Expert@BOS-CKP-FW1:0]# arp -an

? (10.153.99.17) at 00:50:56:A9:64:4B [ether] on eth5.999
? (10.153.99.53) at 00:50:56:01:06:24 [ether] on eth5.999
? (10.153.99.102) at 00:50:56:01:03:13 [ether] on eth5.999
? (10.153.1.8) at 00:2A:6A:A2:7C:C1 [ether] on eth5.1001
? (10.153.99.101) at 00:50:56:01:08:79 [ether] on eth5.999
? (10.153.99.100) at 00:50:56:01:03:0D [ether] on eth5.999
? (10.153.254.1) at 00:00:0C:9F:F0:FE [ether] on Mgmt
? (10.153.99.66) at 00:50:56:01:06:24 [ether] on eth5.999
? (10.153.99.200) at 00:50:56:01:06:32 [ether] on eth5.999
? (10.153.1.20) at 00:0E:B6:AA:91:3E [ether] on eth5.1001
? (10.153.1.10) at 00:00:0C:9F:F3:E9 [ether] on eth5.1001
? (10.153.99.249) at 00:50:56:A9:7E:DE [ether] on eth5.999
? (10.153.99.175) at 00:50:56:A9:36:27 [ether] on eth5.999
```

