# walk through of initial config
- [walk through of initial config](#walk-through-of-initial-config)
  - [Initial Stuff](#initial-stuff)
  - [Interfaces](#interfaces)
  - [Zones](#zones)
  - [Policies](#policies)
  - [NAT](#nat)
  - [Screen](#screen)

Just taking the initial config, and re-sorting it so that we can better see the different sections and how they all interact with each other.  

## Initial Stuff
```
set system autoinstallation delete-upon-commit
set system autoinstallation traceoptions level verbose
set system autoinstallation traceoptions flag all

set system name-server 8.8.8.8
set system name-server 8.8.4.4

set system services ssh
set system services telnet
set system services xnm-clear-text
set system services netconf ssh
set system services dhcp-local-server group jdhcp-group interface irb.0
set system services web-management https system-generated-certificate

set system syslog archive size 100k
set system syslog archive files 3
set system syslog user * any emergency
set system syslog file messages any notice
set system syslog file messages authorization info
set system syslog file interactive-commands interactive-commands any

set system max-configurations-on-flash 5
set system max-configuration-rollbacks 5

set system license autoupdate url https://ae1.juniper.net/junos/key_retrieval
```

## Interfaces
Define external interface, and let the external interface get it's IP from bootp/dhcp
```
set system autoinstallation interfaces ge-0/0/0 bootp

set interfaces ge-0/0/0 unit 0 family inet
```

Group all the internal interfaces
```
set interfaces ge-0/0/1 unit 0 family ethernet-switching vlan members vlan-trust
set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members vlan-trust
set interfaces ge-0/0/3 unit 0 family ethernet-switching vlan members vlan-trust
set interfaces ge-0/0/4 unit 0 family ethernet-switching vlan members vlan-trust
set interfaces ge-0/0/5 unit 0 family ethernet-switching vlan members vlan-trust
set interfaces ge-0/0/6 unit 0 family ethernet-switching vlan members vlan-trust
set interfaces ge-0/0/7 unit 0 family ethernet-switching vlan members vlan-trust

set protocols l2-learning global-mode switching
```
Give the internal interface group a static IP and define it as vlan0 (irb.0)
```
set interfaces irb unit 0 family inet address 192.168.1.1/24
set vlans vlan-trust vlan-id 3
set vlans vlan-trust l3-interface irb.0
```

## Zones
define the external zone.  Allow dhcp and tftp inbound to the external zone interface
```
set security zones security-zone untrust interfaces ge-0/0/0.0 host-inbound-traffic system-services dhcp
set security zones security-zone untrust interfaces ge-0/0/0.0 host-inbound-traffic system-services tftp
```

Define the internal zone.  Allow all protocols to the internal zone's interface.  Also bind that zone to the vlan (irb.0)
```
set security zones security-zone trust host-inbound-traffic system-services all
set security zones security-zone trust host-inbound-traffic protocols all
set security zones security-zone trust interfaces irb.0
```

## Policies
define the outbound flow
```
set security policies from-zone trust to-zone untrust policy trust-to-untrust match source-address any
set security policies from-zone trust to-zone untrust policy trust-to-untrust match destination-address any
set security policies from-zone trust to-zone untrust policy trust-to-untrust match application any
set security policies from-zone trust to-zone untrust policy trust-to-untrust then permit
```

define the flows within the internal zone
```
set security policies from-zone trust to-zone trust policy trust-to-trust match source-address any
set security policies from-zone trust to-zone trust policy trust-to-trust match destination-address any
set security policies from-zone trust to-zone trust policy trust-to-trust match application any
set security policies from-zone trust to-zone trust policy trust-to-trust then permit
```

## NAT
Setup a PAT to allow all internal hosts (with any IP) to use the external interface's IP.  
```
set security nat source rule-set trust-to-untrust from zone trust
set security nat source rule-set trust-to-untrust to zone untrust
set security nat source rule-set trust-to-untrust rule source-nat-rule match source-address 0.0.0.0/0
set security nat source rule-set trust-to-untrust rule source-nat-rule then source-nat interface
```


## Screen
define the screen security session
```
set security screen ids-option untrust-screen icmp ping-death
set security screen ids-option untrust-screen ip source-route-option
set security screen ids-option untrust-screen ip tear-drop
set security screen ids-option untrust-screen tcp syn-flood alarm-threshold 1024
set security screen ids-option untrust-screen tcp syn-flood attack-threshold 200
set security screen ids-option untrust-screen tcp syn-flood source-threshold 1024
set security screen ids-option untrust-screen tcp syn-flood destination-threshold 2048
set security screen ids-option untrust-screen tcp syn-flood timeout 20
set security screen ids-option untrust-screen tcp land
```

deploy the screen to the external zone
```
set security zones security-zone untrust screen untrust-screen 
```

