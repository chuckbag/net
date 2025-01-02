# Ports, Trunks and Port-Channels

## Basic Port
Make sure the VLAN is defined: 

```
vlan 150
 name myserversvlan
```

Then define the port: 

```
interface GigabitEthernet0/5
 description my server
 switchport access vlan 150
 switchport mode access
 spanning-tree portfast
```

## Trunk Port
The basic config would be: 

```
interface GigabitEthernet0/39
 description My other switch
 switchport access vlan 3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 801-803
 switchport mode trunk
 logging event trunk-status
```

### Adding to large lists of allowed vlans: 
Sometimes the list of allowed vlans on a trunk interface can be very large.  IOS modifies this list such that each line is not too long with the vlan add phrase.  If you need to add additional vlans, put them ALL in a single command and enter them, or use the vlan add command.  This is demonstrated in the LAB: adding large strings of vlans.  

## Trunks with Port Channel
For this example, we want two physical cables to behave as one, and "share" bandwidth across both links. We do this with "[etherchannel](http://en.wikipedia.org/wiki/EtherChannel)" or [lacp](http://en.wikipedia.org/wiki/Link_Aggregation_Control_Protocol#Link_Aggregation_Control_Protocol).   

First Interface: 

```
interface GigabitEthernet0/1
 description test uplink 1
 switchport access vlan 3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 3,801-802
 switchport mode trunk
 logging event trunk-status
 carrier-delay msec 0
 no keepalive
 channel-group 4 mode active
```

Second Interface: 

```
interface GigabitEthernet0/2
 description test uplink 2
 switchport access vlan 3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 3,801-802
 switchport mode trunk
 logging event trunk-status
 carrier-delay msec 0
 no keepalive
 channel-group 4 mode active
```

Port Channel: 

```
interface Port-channel4
 description Uplink: test portchannel 3 
 switchport access vlan 3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 2
 switchport trunk allowed vlan 3,801-802
 switchport mode trunk
 logging event trunk-status
 spanning-tree cost 20
```

### Updating allowed VLANs on a Port Channel: 
You need to make sure that the allowed vlans always match.  If you modify one of the interfaces vlan lists, it will break the port channel.  To make modifications, do it to the port-channel ONLY.  It will update the bound interfaces automatically.    See: LAB: updating vlans on a port-channel 

```
! only make the vlan update on the port-channel, not the interfaces !
conf t
interface Port-channel3
 switchport trunk allowed vlan 3,801-803
end
```

