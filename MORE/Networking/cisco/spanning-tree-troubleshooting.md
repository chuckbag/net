# spanning tree - troubleshooting

Check all your switches and make sure that the spanning tree mode is the same. 

```
BOS-2960PDU-SW1#show spanning-tree summary 

Switch is in rapid-pvst mode
Root bridge for: none
EtherChannel misconfig guard is enabled
Extended system ID           is enabled
Portfast Default             is disabled
PortFast BPDU Guard Default  is disabled
Portfast BPDU Filter Default is disabled
Loopguard Default            is disabled
UplinkFast                   is disabled
BackboneFast                 is disabled
Configured Pathcost method used is short
Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
VLAN0010                     0         0        0          5          5
VLAN0033                     0         0        0          1          1
VLAN0034                     0         0        0          1          1
```

make sure that all the vlans are being properly sent out the trunk interfaces

```
BOS-2960PDU-SW1#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Po1         on               802.1q         trunking      900
Po48        on               802.1q         trunking      900
Port        Vlans allowed on trunk
Po1         10,64-71
Po48        10,33-40,50,64-71,1001
Port        Vlans allowed and active in management domain
Po1         10,64-71
Po48        10,33-40,50,64-71,1001
Port        Vlans in spanning tree forwarding state and not pruned
Po1         10,64-71
Po48        10,33-40,50,64-71,1001

BOS-2960PDU-SW1#
```

