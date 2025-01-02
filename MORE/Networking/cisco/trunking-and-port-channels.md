# Trunking and Port Channels

## Basic Trunk Interface: 

```
!
interface GigabitEthernet2/0/24
 description Uplink: uplink to something special
 switchport access vlan 3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 3,50-53
 switchport mode trunk
 logging event trunk-status
 spanning-tree cost 10
!
```

Where `switchport trunk allowed` specifies which vlans are allowed over the trunk, and `spanning-tree cost 10` hard codes the cost of the link.  (if you have a redundant link from a different switch, set it to `20`, and it will always be the blocked link.)

## Port Channel Interface: 

Define the port channel interface

```
interface Port-channel5
 description Uplink: access-switch-1
 switchport access vlan 2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 249,501-503,599,753,754
 switchport mode trunk
 logging event trunk-status
 spanning-tree cost 10
!
```

The vlans allowed MUST be the exact same for both.  When modifying, change the interfaces before the port channel, otherwise the link will fail.  

Define the uplink ports: 

```
interface GigabitEthernet1/0/17
 description Uplink: access-switch-1 port g0/1
 switchport access vlan 2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 249,501-503,599,753,754
 switchport mode trunk
 logging event trunk-status
 carrier-delay msec 0
 no keepalive
 channel-group 5 mode active
!
interface GigabitEthernet2/0/17
 description Uplink: access-switch-1 port g0/2
 switchport access vlan 2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 249,501-503,599,753,754
 switchport mode trunk
 logging event trunk-status
 carrier-delay msec 0
 no keepalive
 channel-group 5 mode active
```

Note that the channel-group setting can either be `on` or `active`.  if you are ever having problems setting up the channel, set this to `on`.  