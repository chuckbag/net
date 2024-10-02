
**Table of Contents**
- [Minecraft Server](#minecraft-server)
  - [Overview:](#overview)
  - [Review](#review)
    - [Network topo:](#network-topo)
    - [Inbound server ports](#inbound-server-ports)
  - [Config Changes:](#config-changes)
    - [Address Books](#address-books)
    - [Policy](#policy)
    - [outbound NAT (from the server out to the internet)](#outbound-nat-from-the-server-out-to-the-internet)
    - [inbound NAT (to the server)](#inbound-nat-to-the-server)


# Minecraft Server

## Overview: 
Configuring a Juniper SRX to support a minecraft server on the backend.  Yea... It's kind of silly, but it's a good example to show address-books, policies, and source/destination NATs.  

## Review
First lets have a look at the firewall config.  We're going to ignore the configs for the inside and dmz network, as those configs are defined elsewhere (not in this example).

### Network topo: 
The environment is wired up like the following. The physical configurations are already done for this test, but the rules for the web tier are not, and will be covered below.  Also, the internet interface (dmz) is setup not with a static IP, but with a DHCP IP (like as if it were plugged into a home internet provider, or your standard network that provides DHCP IPs.) 
```
{internet}
    | 
    | (zone: dmz)
    | (vlan 10)
    | (dhcp)
    | (ge-0/0/0.10)
{firewall}-------------------------\
    | (ge-0/0/0.11)                | (ge-0/0/0.12)
    | (198.18.1.1/24)              | (198.18.2.1/24)
    | (vlan.11)                    | (vlan.12)
    | (zone: web)                  | (zone: inside)
    |                              |
    | (198.18.1.10)                | (198.18.2.x)
{server}                      {home laptop}
```
### Inbound server ports
Inbound traffic to the minecraft server needs to be: 

```
world -(tcp:25565)-> server
```

## Config Changes: 
To make all this work, we will need to make the following changes: 

### Address Books
We want to reference things properly, so first lets define reference names for things.   Since we're worried about bad guys messing up our server, we will only whitelist IP ranges (ie: "Friendlies") who can access the server.

```
set security address-book global address minecraftserver 198.18.1.10/32
set security address-book global address vzn 74.96.0.0/12
set security address-book global address comcast73 73.0.0.0/8
set security address-book global address att10764 107.64.0.0/10

set security address-book global address-set friendlies address vzn
set security address-book global address-set friendlies address comcast73
set security address-book global address-set friendlies address comcast73

set applications application minecraft-java protocol tcp
set applications application minecraft-java destination-port 25565
```

### Policy
Since this is a new zone (the web one), we will need to set a policy allowing it outbound access.  Without this, the server would not be able to connect out to the internet.  (For a server, you really want to lock it down, and only let it talk to what it needs to talk to, but for now, lets just keep the doors wide open.)
```
set security policies from-zone web to-zone dmz policy all-out match source-address net-web
set security policies from-zone web to-zone dmz policy all-out match destination-address any
set security policies from-zone web to-zone dmz policy all-out match application any
set security policies from-zone web to-zone dmz policy all-out then permit
```


We also need to make sure that folks from the internet can connect to the minecraft server, and also folks from the inside network. These changes will allow those flows.  Notice that from the internet, we only want to open up the minecraft-java ports, but from the inside, we also walt to let the users be able to ssh to the server.  

```
set security policies from-zone dmz to-zone web policy minecraft match source-address friendlies
set security policies from-zone dmz to-zone web policy minecraft match destination-address minecraftserver
set security policies from-zone dmz to-zone web policy minecraft match application minecraft-java
set security policies from-zone dmz to-zone web policy minecraft then permit
set security policies from-zone dmz to-zone web policy minecraft then count

set security policies from-zone inside to-zone web policy minecraft match source-address net-inside
set security policies from-zone inside to-zone web policy minecraft match destination-address minecraftserver
set security policies from-zone inside to-zone web policy minecraft match application minecraft-java
set security policies from-zone inside to-zone web policy minecraft match application junos-ping 
set security policies from-zone inside to-zone web policy minecraft match application junos-ssh
set security policies from-zone inside to-zone web policy minecraft then permit
set security policies from-zone inside to-zone web policy minecraft then count
```

### outbound NAT (from the server out to the internet)
The minecraft server will need outbound access for updates and stuff.  It is on a private IP range (198.18.1.0/24), but it will need to use the IP of the DMZ interface on the firewall.   This is generally called a PAT translation or with Juniper, a "Source NAT".  To set this up, we will need to enter in the following changes: 

```
set security nat source rule-set natWeb2dmz from zone web
set security nat source rule-set natWeb2dmz to zone dmz

set security nat source rule-set natWeb2dmz rule patOut3 match source-address-name net-web
set security nat source rule-set natWeb2dmz rule patOut3 then source-nat interface
```

### inbound NAT (to the server)
Now we need to configure a NAT that will alow traffic sent to the public IP on the firewalls DMZ network to be re-routed to the minecraft server.  In Juniper-talk, this is called a "Destination NAT", and includes the following commands.  

```
set security nat destination rule-set public_IN from zone dmz

set security nat destination rule-set public_IN from zone dmz
set security nat destination rule-set public_IN rule minecraft match destination-address 0.0.0.0/0
set security nat destination rule-set public_IN rule minecraft match destination-port 25565
set security nat destination rule-set public_IN rule minecraft then destination-nat pool minecraftServer

set security nat destination pool minecraftServer address 198.18.1.10/32
set security nat destination pool minecraftServer address port 25565


delete security nat proxy-arp interface ge0/0/0.10 address 0.0.0.0/0

```