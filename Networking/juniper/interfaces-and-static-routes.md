# Interfaces and Static Routes

- [Interfaces and Static Routes](#interfaces-and-static-routes)
  - [What interfaces are there?:](#what-interfaces-are-there)
  - [Configure a basic interface](#configure-a-basic-interface)
  - [Configure a basic trunked interfaces](#configure-a-basic-trunked-interfaces)
    - [Add VRRP](#add-vrrp)
  - [Static Route:](#static-route)
  - [Switched Interfaces with VLANs](#switched-interfaces-with-vlans)
  - [Advanced Interfaces](#advanced-interfaces)
    - [Physical Interfaces](#physical-interfaces)
    - [Interface groups](#interface-groups)
    - [VLANs](#vlans)
    - [Zones](#zones)
  - [References:](#references)

## What interfaces are there?:
From the CLI mode (not in edit) you can simply view all the interfaces with the interface terse command.   Note that this shows *all* the interfaces on the router, some of them are physical, some logical, and some for internal usage only.  The list is somewhat confusing, but you should focus on the following kinds of interfaces for the time being: 

- `xe-0/0/port/` 	 Configure a 10-Gigabit Ethernet interface.
- `ge-0/0/port/`	 Configure a Gigabit Ethernet interface
- `lo0`	 Loopback interface
- `meX/`	 Configure a management interface

```bash 
root> show interfaces terse
Interface               Admin Link Proto    Local                 Remote
ge-0/0/0                up    up
ge-0/0/0.0              up    up
gr-0/0/0                up    up
ip-0/0/0                up    up
lsq-0/0/0               up    up
lt-0/0/0                up    up
mt-0/0/0                up    up
ge-0/0/1                up    up
ge-0/0/2                up    down
ge-0/0/3                up    down
ge-1/0/0                up    down
dsc                     up    up
gre                     up    up
ipip                    up    up
lo0                     up    up
lo0.16384               up    up   inet     127.0.0.1           --> 0/0
lo0.16385               up    up   inet     10.0.0.1            --> 0/0
                                            10.0.0.16           --> 0/0
                                            128.0.0.1           --> 0/0
                                            128.0.1.16          --> 0/0
                                   inet6    fe80::205:860f:fc71:db00
lo0.32768               up    up
lsi                     up    up
mtun                    up    up
pimd                    up    up
pime                    up    up
pp0                     up    up
ppd0                    up    up
ppe0                    up    up
st0                     up    up
tap                     up    up
vlan                    up    down

root>
```

## Configure a basic interface
In this example, we are enabling the interface in slot 1, defining its IP, and providing a name for it.  
```bash
root# set interfaces ge-1/0/0 unit 0 family inet address 5.5.5.5/30
root# set interfaces ge-1/0/0 description "external border link"
```

## Configure a basic trunked interfaces
A standard trunked interface allows you to transport multiple ethernet broadcast domains over one physical interfaces.  To do this we simply enable vlan-tagging on an interface, define a default (unit0) vlan, and then define all the additional interfaces that need to be on that link.  Note that the default tagging protocol is 802.1q, and is not needed to be defined.  

First we enable vlan-tagging on the interface, and then define the default vlan for unit 0.  
```bash
set interfaces ge-0/0/1 vlan-tagging
set interfaces ge-0/0/1 unit 0 vlan-id 2
set interfaces ge-0/0/1 unit 0 description "default vlan inteface"
```

Next we create our first vlan for the network, and define a SVI for that vlan.  
```bash
set interfaces ge-0/0/1 unit 801 vlan-id 801 
set interfaces ge-0/0/1 unit 801 family inet address 6.6.6.2/27
set interfaces ge-0/0/1 unit 801 description "b1-trans1, vlan-801, vpn public ip range"
```

We can repeat the previous step as many times as needed to create multiple vlans.  
```bash
set interfaces ge-0/0/1 unit 802 vlan-id 802 
set interfaces ge-0/0/1 unit 802 family inet address 10.10.10.2/23
set interfaces ge-0/0/1 unit 802 description "b1-trans2, vlan-802, private IP range"
```

### Add VRRP
If you need to setup VRRP on a trunked interface, you simply add that to the already defined interface as shown.  Note that the priority number defines which router will default as the primary.  The higher priority number is the master.
``` 
set interfaces ge-0/0/1 unit 801 family inet address 6.6.6.2/27 vrrp-group 1 virtual-address 199.233.202.1
set interfaces ge-0/0/1 unit 801 family inet address 6.6.6.2/27 vrrp-group 1 priority 101
set interfaces ge-0/0/1 unit 801 family inet address 6.6.6.2/27 vrrp-group 1 authentication-type md5
set interfaces ge-0/0/1 unit 801 family inet address 6.6.6.2/27 vrrp-group 1 authentication-key RealyBadKey
set interfaces ge-0/0/1 unit 801 family inet address 6.6.6.2/27 vrrp-group 1 accept-data
```

A second interface would be defined the same way.  
```
set interfaces ge-0/0/1 unit 802 family inet address 10.10.10.2/23   vrrp-group 2 virtual-address 10.50.10.1
set interfaces ge-0/0/1 unit 802 family inet address 10.10.10.2/23   vrrp-group 2 priority 101
set interfaces ge-0/0/1 unit 802 family inet address 10.10.10.2/23   vrrp-group 2 authentication-type md5
set interfaces ge-0/0/1 unit 802 family inet address 10.10.10.2/23   vrrp-group 2 authentication-key ReallyBadCay
set interfaces ge-0/0/1 unit 802 family inet address 10.10.10.2/23   vrrp-group 2 accept-data
```



## Static Route:
A static, default, route is entered with the 0.0.0.0/0 (all) notation:
```
set routing-options static route 0.0.0.0/0 next-hop 6.6.6.1
```

## Switched Interfaces with VLANs
Rather then assigning IP's to specific interfaces, we can also assign IPs to VLAN SVI's and then assign ports to those vlans.  This is helpful if you want to have two or more uplinks all on the same SVI (like uplinks to switches).  

Below shows how to do this with v.15 JunOS.  The older versions of JunOS have a different method.  

Configure ethernet switching on node0:
```
set interfaces ge-0/0/3 unit 0 family ethernet-switching interface-mode access
set interfaces ge-0/0/4 unit 0 family ethernet-switching interface-mode access
```

create vlan 100:
```
set vlans vlan100 vlan-id 100
```

add interfaces to vlan: 
```
set interfaces ge-0/0/3 unit 0 family ethernet-switching vlan members vlan100
set interfaces ge-0/0/4 unit 0 family ethernet-switching vlan members vlan100
```

create a logical interface (SVI), and link it to a vlan
```
set interfaces irb unit 100 family inet address 192.0.2.100/24
set vlans vlan100 l3-interface irb.100
```

Save:
```
show | compare | no-more
commit check
commit and-quit
```

## Advanced Interfaces
Here we're setting up two ports on the firewall that are configured the same, and both of them are trunked interfaces.  (like two connections going to two redundant switches.)

### Physical Interfaces

Configure the interface
```
set interfaces ge-0/0/4 unit 0
set interfaces ge-0/0/5 unit 0
```

### Interface groups

Bind the ports together
```
set interfaces interface-range mgmtGroup member ge-0/0/4
set interfaces interface-range mgmtGroup member ge-0/0/5
```

enable tagging on the bound group
```
set interfaces interface-range mgmtGroup unit 0 family ethernet-switching interface-mode trunk
```

add a zone to the group
```
set interfaces interface-range mgmtGroup unit 0 family ethernet-switching vlan members mgmt5
```

### VLANs

Define the vlan @ layer 2
```
set vlans mgmt5 description "admin (full access)"
set vlans mgmt5 vlan-id 36
set vlans mgmt5 l3-interface irb.36
```

Define the vlan @ layer 3
```
set interfaces irb unit 36 description "mgmt5 - admin (full access)"
set interfaces irb unit 36 family inet address 10.36.36.1/24
```

### Zones

Define the zone
```
set security zones security-zone mgmt5 interfaces irb.36 host-inbound-traffic system-services dhcp
set security zones security-zone mgmt5 interfaces irb.36 host-inbound-traffic system-services ping
set security zones security-zone mgmt5 interfaces irb.36 host-inbound-traffic system-services ssh
```


## References: 
- [Interface Naming Overview](http://www.juniper.net/techpubs/en_US/junos/topics/concept/interfaces-interface-naming-overview.html): Juniper, 13 Feb, 2013
- [Using the Enhanced Layer 2 Software CLI](https://www.juniper.net/documentation/en_US/junos/topics/task/configuration/getting-started-els.html): Juniper, May, 2017
- [Example: Configuring IRB and VLAN with Members Across Two Nodes on a Security Device (CLI Procedure)](https://www.juniper.net/documentation/en_US/junos/topics/example/chassis-cluster-vlan-irb-configuring-cli.html): Juniper, Oct 2017
- [Example – Configure Transparent-Bridging in SRX](https://kb.juniper.net/InfoCenter/index?page=content&id=KB31081): Juniper, KB31081, Aug 2017



