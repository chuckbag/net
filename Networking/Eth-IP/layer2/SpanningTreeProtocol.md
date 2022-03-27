# Spanning Tree Protocol

- [Spanning Tree Protocol](#spanning-tree-protocol)
  - [Spanning Tree Overview:](#spanning-tree-overview)
    - [Root Switches](#root-switches)
    - [Limitations on spanning tree sizes](#limitations-on-spanning-tree-sizes)
    - [BPDU Messages](#bpdu-messages)
    - [STP Switch Port States](#stp-switch-port-states)
  - [Advances in Spanning Tree:](#advances-in-spanning-tree)
    - [Cisco Special Sauce](#cisco-special-sauce)
    - [Rapid Spanning Tree Protocol (RSTP)](#rapid-spanning-tree-protocol-rstp)
      - [Backwards Comparability with STP](#backwards-comparability-with-stp)
      - [RSTP Port Roles and States](#rstp-port-roles-and-states)
    - [Per-VLAN Spanning Tree (PVST)](#per-vlan-spanning-tree-pvst)
    - [Multiple Instance Spanning Tree Protocol (MISTP)](#multiple-instance-spanning-tree-protocol-mistp)
    - [Rapid Per-VLAN Spanning Tree (R-PVST)](#rapid-per-vlan-spanning-tree-r-pvst)
  - [Configuring Spanning Tree](#configuring-spanning-tree)
- [References:](#references)

## Spanning Tree Overview:
Spanning-Tree Protocol (STP) prevents loops from being formed when switches or bridges are interconnected via multiple paths. Spanning-Tree Protocol implements the [802.1D IEEE](http://www.ieee802.org/1/pages/802.1D.html) algorithm by exchanging BPDU messages with other switches to detect loops, and then removes the loop by shutting down selected bridge interfaces. This algorithm guarantees that there is one and only one active path between two network devices.


### Root Switches
One of the switches must select a root bridge. 

### Limitations on spanning tree sizes

### BPDU Messages

### STP Switch Port States
When first powered on, each port goes through 4 states to ensure that there are no physical loops in the layer 2 broadcast domain.  These steps are outlined as follows.  With the initial version of spanning tree, this process could take from 30-60 seconds. 

1. **Blocking** - A port that would cause a switching loop, no user data is sent or received but it may go into forwarding mode if the other links in use were to fail and the spanning tree algorithm determines the port may transition to the forwarding state. BPDU data is still received in blocking state.
2. **Listening** - The switch processes BPDUs and awaits possible new information that would cause it to return to the blocking state.
3. **Learning** - While the port does not yet forward frames (packets) it does learn source addresses from frames received and adds them to the filtering database (switching database)
4. **Forwarding** - A port receiving and sending data, normal operation. STP still monitors incoming BPDUs that would indicate it should return to the blocking state to prevent a loop.

## Advances in Spanning Tree:
Common Spanning-Tree (CST) assumes one spanning-tree instance for the entire bridged network, regardless of the number of VLANs. This implementation reduces CPU load since only one Spanning Tree instance is maintained for the entire network. This implementation can be used when only one Layer 2 topology is needed in the network

STP Default Configuration:

| Feature	| Default Value |
|--|--|
Enable state	| STP enabled for all VLANs
Bridge priority| 	32768
STP port priority <br> (configurable on a per-port basis—used on LAN ports configured as Layer 2 access ports)	| 128
STP port cost<br>(configurable on a per-port basis—used on LAN ports configured as Layer 2 access ports)	| 10-Gigabit Ethernet: 2<br>Gigabit Ethernet: 4<br>Fast Ethernet: 19<br>Ethernet: 100
STP VLAN port priority<br>(configurable on a per-VLAN basis—used on LAN ports configured as Layer 2 trunk ports)	|128
STP VLAN port cost<br>(configurable on a per-VLAN basis—used on LAN ports configured as Layer 2 trunk ports)	| 10-Gigabit Ethernet: 2<br>Gigabit Ethernet: 4<br>Fast Ethernet: 19<br>Ethernet: 100
Hello time	| 2 seconds
Forward delay time	| 15 seconds
Maximum aging time	| 20 seconds
Mode	| PVST

### Cisco Special Sauce

If a loop occurs, STP considers port priority when selecting a LAN port to put into the forwarding state. You can assign higher priority values to LAN ports that you want STP to select first and lower priority values to LAN ports that you want STP to select last. If all LAN ports have the same priority value, STP puts the LAN port with the lowest LAN port number in the forwarding state and blocks other LAN ports. The possible priority range is 0 through 240 (default 128), configurable in increments of 16.

```
! --- Setting port priority to 160 from 128
Router# configure terminal
Router(config)# interface fastethernet 4/4
Router(config-if)# spanning-tree port-priority 160
Router(config-if)# end
```

Rapid connectivity is established only on point-to-point links.  The switch assumes that all full-duplex links are point-to-point links and that half-duplex links are shared links, you can avoid explicitly configuring the link type. To configure a specific link type, enter the spanning-tree linktype command.

```
! --- Enabling RSTP
Router# config t
Router(config)# spanning-tree mode rapid-pvst
Router(config)# interface fastethernet 4/4
Router(config-if)# spanning-tree linktype !----- <- finish
Router(config)# end
```

When configured globally, PortFast BPDU filtering applies to all operational PortFast ports. Ports in an operational PortFast state are supposed to be connected to hosts, that typically drop BPDUs. If an operational PortFast port receives a BPDU, it immediately loses its operational PortFast status. In that case, PortFast BPDU filtering is disabled on this port and STP resumes sending BPDUs on this port.

STP PortFast causes a Layer 2 LAN interface configured as an access port to enter the forwarding state immediately, bypassing the listening and learning states. You can use PortFast on Layer 2 access ports connected to a single workstation or server to allow those devices to connect to the network immediately, instead of waiting for STP to converge. Interfaces connected to a single workstation or server should not receive bridge protocol data units (BPDUs). When configured for PortFast, a port is still running the spanning tree protocol. A PortFast enabled port can immediately transition to the blocking state if necessary (this could happen on receipt of a superior BPDU).

When enabled on a port, BPDU Guard shuts down a port that receives a BPDU. When configured globally, BPDU Guard is only effective on ports in the operational PortFast state. In a valid configuration, PortFast Layer 2 LAN interfaces do not receive BPDUs. Reception of a BPDU by a PortFast Layer 2 LAN interface signals an invalid configuration, such as connection of an unauthorized device. BPDU Guard provides a secure response to invalid configurations, because the administrator must manually put the Layer 2 LAN interface back in service. With release 12.1(11b)E, BPDU Guard can also be configured at the interface level. When configured at the interface level, BPDU Guard shuts the port down as soon as the port receives a BPDU, regardless of the PortFast configuration.

```
! --- how to enable PortFast on Fast Ethernet interface 5/8:
Router# configure terminal
Router(config)# interface fastethernet 5/8
Router(config-if)# spanning-tree portfast
Router(config-if)# end
Router#
```

This example shows how to verify the configuration:
```
Router# show running-config interface fastethernet 5/8
Building configuration...

current configuration:
!
interface FastEthernet5/8
 no ip address
 switchport
 switchport access vlan 200
 switchport mode access
 spanning-tree portfast
end

Router#
```

How to enable portfast
```
! --- How to enable the default PortFast
Router# configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)# spanning-tree portfast default
Router(config)# ^Z
Router# show spanning-tree summary totals
Root bridge for:VLAN0010
EtherChannel misconfiguration guard is enabled
Extended system ID   is disabled
Portfast             is enabled by default
PortFast BPDU Guard  is disabled by default
Portfast BPDU Filter is disabled by default
Loopguard            is disabled by default
UplinkFast           is disabled
BackboneFast         is disabled
Pathcost method used is long

Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
VLAN0001                  0        0         0        1          1     
VLAN0010                  0        0         0        2          2     
---------------------- -------- --------- -------- ---------- ----------
2 vlans                   0        0         0        3          3
Router#
```

Enabling default BPDU Guard
```
! --- How to enable BPDU Guard
Router(config)# spanning-tree portfast bpdufilter default
Router(config)# ^Z

Router# show spanning-tree summary totals
Root bridge for:VLAN0010
EtherChannel misconfiguration guard is enabled
Extended system ID   is disabled
Portfast             is enabled by default
PortFast BPDU Guard  is disabled by default
Portfast BPDU Filter is enabled by default
Loopguard            is disabled by default
UplinkFast           is disabled
BackboneFast         is disabled
Pathcost method used is long

Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
2 vlans                   0        0         0        3          3     
Router#
```

### Rapid Spanning Tree Protocol (RSTP)
Rapid Spanning Tree Protocol (RSTP) is an evolution of the Spanning Tree Protocol ([802.1D standard](https://www.ieee802.org/1/pages/802.1w.html)) and provides for faster spanning tree convergence after a topology change. The standard also includes features equivalent to Cisco PortFast, UplinkFast and BackboneFast for faster network reconvergence.

#### Backwards Comparability with STP
RSTP provides backward compatibility with 802.1D bridges as follows:
1. RSTP selectively sends 802.1D-configured BPDUs and topology change notification (TCN) BPDUs on a per-port basis.
2. When a port initializes, the migration-delay timer starts and RSTP BPDUs are transmitted. While the migration-delay timer is active, the bridge processes all BPDUs received on that port.
3. If the bridge receives an 802.1D BPDU after a port's migration-delay timer expires, the bridge assumes it is connected to an 802.1D bridge and starts using only 802.1D BPDUs.
4. When RSTP uses 802.1D BPDUs on a port and receives an RSTP BPDU after the migration-delay expires, RSTP restarts the migration-delay timer and begins using RSTP BPDUs on that port.

#### RSTP Port Roles and States

RSTP uses the following definitions for port roles:
1. **Root** — A forwarding port elected for the spanning tree topology.
2. **Designated** — A forwarding port elected for every switched LAN segment.
3. **Alternate** — An alternate path to the root bridge to that provided by the current root port.
4. **Backup** — A backup for the path provided by a designated port toward the leaves of the spanning tree. Backup ports can exist only where two ports are connected together in a loopback by a point-to-point link or bridge with two or more connections to a shared LAN segment.
5. **Disabled** — A port that has no role within the operation of spanning tree.

Port roles are assigned as follows:

1. A root port or designated port role includes the port in the active topology.
2. An alternate port or backup port role excludes the port from the active topology.

Comparison between STP and RSTP Port States:

| Phase | Operational Status | STP Port State |  RSTP Port State |  Port Included in Active Topology | 
|--|--|--|--|--|
 1	| Enabled	| Blocking   | Discarding | No
 2	| Enabled	| Listening	 | Discarding | No
 3	| Enabled	| Learning	 | Learning   | Yes
 4	| Enabled	| Forwarding | Forwarding | Yes
 &nbsp; | Disabled | Disabled | Discarding | No



### Per-VLAN Spanning Tree (PVST)
Per-VLAN Spanning Tree (**PVST**) maintains a spanning tree instance for each VLAN configured in the network. It uses ISL Trunking and allows a VLAN trunk to be forwarding for some VLANs while blocking for other VLANs. Since PVST treats each VLAN as a separate network, it has the ability to load balance traffic (at layer-2) by forwarding some VLANs on one trunk and other Vlans on another trunk without causing a Spanning Tree loop.

Per VLAN Spanning Tree Plus (**PVST+**) provides the same functionality as PVST using 802.1Q trunking technology rather than ISL. PVST+ is an enhancement to the 802.1Q specification and is not supported on non-Cisco devices.

### Multiple Instance Spanning Tree Protocol (MISTP)
MISTP  implements the [802.1s IEEE](http://www.ieee802.org/1/pages/802.1s.html) standard which allows several VLANs to be mapped to a reduced number of spanning-tree instances. This is possible since most networks do not need more than a few logical topologies. Each instance handles multiple VLANs that have the same Layer 2 topology.

### Rapid Per-VLAN Spanning Tree (R-PVST)


## Configuring Spanning Tree
```
Router# configure terminal
Router(config)# spanning-tree vlan 200
Router(config)# end
Router#


Router# show spanning-tree vlan 200

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    32768
             Address     00d0.00b8.14c8
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32768
             Address     00d0.00b8.14c8
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300

Interface        Role Sts Cost      Prio.Nbr Status
---------------- ---- --- --------- -------- --------------------------------
Fa4/4            Desg FWD 200000    128.196  P2p
Fa4/5            Back BLK 200000    128.197  P2p

Router#
```

Catalyst 6500 series switches maintain a separate instance of STP for each active VLAN. A bridge ID, consisting of the bridge priority and the bridge MAC address, is associated with each instance. For each VLAN, the network device with the lowest bridge ID becomes the root bridge for that VLAN.

To configure a VLAN instance to become the root bridge, enter the `spanning-tree vlan vlan_ID root` command to modify the bridge priority from the default value (32768) to a significantly lower value.



# References: 
- [9 Common Spanning Tree Mistakes](https://www.networkworld.com/article/2223757/cisco-subnet-9-common-spanning-tree-mistakes.html): Network World, Jan 2, 2013
- [Understanding and Configuring Spanning Tree Protocol (STP) on Catalyst Switches](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/5234-5.html): Cisco, Aug 17, 2006
