# nxos spanning-tree

For spanning tree, you can enable either Rapid per VLAN Spanning Tree (Rapid PVST+) or Multiple Spanning Tree.  Rapid PVST+ is the devfault, but I like Multiple Spanning Tree, MST, (802.1s) better as it enables you to assign multiple vlans to the same spanning tree instance. 

```
spanning-tree mode mst
spanning-tree port type edge bpduguard default
spanning-tree loopguard default
spanning-tree mst configuration
  name sl-mgmt
  revision 1
  instance 1 vlan 1-3499,3900-3967,4048-4093
  instance 2 vlan 3500-3799
  instance 3 vlan 3800-3899
interface port-channel1
  description sl-csw01
  switchport mode trunk
  switchport access vlan 3
  switchport trunk native vlan 2
  switchport trunk allowed vlan 4-3967,4048-4093
  spanning-tree cost 1
interface Ethernet1/1
  description sl-csw01-te4/2
  switchport mode trunk
  switchport access vlan 3
  switchport trunk native vlan 2
  switchport trunk allowed vlan 4-3967,4048-4093
  channel-group 1 mode active
```

- (`spanning-tree mode mst`) enables the multiple spanning tree config mode.  This basically enables spanning tree
- (`spanning-tree port type edge bpduguard default`) enables bpdu guard as a default on all edge ports.
- (`spanning-tree loopguard default`) enables loopguard as a default on all point-to-point links (not edge points).
- (`spanning-tree mst configuration`) start of  mst config mode.
- (`spanning-tree cost`) set the weight of the link for spanning tree.  Higher value is a higher cost, and less desirable. 

## References:
- [Configuring Multiple Spanning Tree, Cisco Nexus 5000 Series NX-OS Layer 2 Switching Configuring Guide](http://www.cisco.com/en/US/customer/docs/switches/datacenter/nexus5000/sw/layer2/421_n1_1/Cisco_n5k_layer2_config_gd_rel_421_n1_1_chapter10.html), Release 4.2(1)N1(1)