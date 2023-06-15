# Spanning Tree




```
nee-swc1c04#sh spanning-tree vlan 51
Spanning tree instance for vlan 51
MST0
  Spanning tree enabled protocol rstp
  Root ID    Priority    32768
             Address     2a99.3a1f.1a01
             This bridge is the root

  Bridge ID  Priority    32768  (priority 32768 sys-id-ext 0)
             Address     2a99.3a1f.1a01
             Hello Time  2.000 sec  Max Age 20 sec  Forward Delay 15 sec

Interface        Role       State      Cost      Prio.Nbr Type
---------------- ---------- ---------- --------- -------- --------------------
Et1              designated forwarding 2000      128.1    P2p Edge
Et2              designated forwarding 2000      128.2    P2p Edge
Et3              designated forwarding 2000      128.3    P2p Edge
Et4              designated forwarding 2000      128.4    P2p Edge
Et5              designated forwarding 2000      128.5    P2p Edge
Et6              designated forwarding 2000      128.6    P2p Edge
Et7              designated forwarding 2000      128.7    P2p Edge
Et8              designated forwarding 2000      128.8    P2p Edge
Et9              designated forwarding 2000      128.9    P2p Edge
Et10             designated forwarding 2000      128.10   P2p Edge
Et13             designated forwarding 2000      128.13   P2p Edge
Et14             designated forwarding 2000      128.14   P2p Edge
Et15             designated forwarding 2000      128.15   P2p Edge
Et16             designated forwarding 2000      128.16   P2p Edge
Et17             designated forwarding 2000      128.17   P2p Edge
Et18             designated forwarding 2000      128.18   P2p Edge
```

shows the priority set for the vlan, and who is the root.  Also lists all of the ethernet interfaces that are included in this vlan and its bridge.
