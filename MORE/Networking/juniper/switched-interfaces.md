# switched interfaces

## Overview
In the more modern versions, "vlan" is replaced by "irb".  

## Changes

### Define VLANs
```
set vlans vlan-10 vlan-id 10
set vlans vlan-10 l3-interface vlan.10
set vlans vlan-11 vlan-id 11
set vlans vlan-11 l3-interface vlan.11
```

### Define SVI's
```
set interfaces vlan unit 10 description "prod-1 : work area one"
set interfaces vlan unit 10 family inet address 10.33.64.1/24
set interfaces vlan unit 11 description "prod-2 : work area two"
set interfaces vlan unit 11 family inet address 10.33.65.1/24
```

### Link Interfaces
```
set interfaces interface-range prodSwitch member fe-0/0/4
set interfaces interface-range prodSwitch member fe-0/0/5
```

### Bind VLAN to linked interfaces
```
set interfaces interface-range prodSwitch unit 0 family ethernet-switching vlan members vlan-10
set interfaces interface-range prodSwitch unit 0 family ethernet-switching vlan members vlan-11
```

### define the zones
```
set security zones security-zone prod1 interfaces vlan.10
set security zones security-zone prod2 interfaces vlan.11
```
### add VLAN tagging on the interfaces
```
set interfaces fe-0/0/4 unit 0 family ethernet-switching vlan members vlan-10
set interfaces fe-0/0/4 unit 0 family ethernet-switching vlan members vlan-11

set interfaces fe-0/0/5 unit 0 family ethernet-switching vlan members vlan-10
set interfaces fe-0/0/5 unit 0 family ethernet-switching vlan members vlan-11
```

(If your using irb's, you don't need to do this step)
but you would need to have the following: 
```
set interfaces fe-0/0/4 unit 0
set interfaces fe-0/0/5 unit 0
```

### Save and Quit
```
show | compare | no-more
commit check
commit and quit
```


## References: 
- [SRX Getting Started - Configure Ethernet ports for switching](https://kb.juniper.net/InfoCenter/index?page=content&id=KB16667&actp=METADATA): Juniper  KB16667, May 2014
- [Only unit 0 is valid for this encapsulation + SRX](https://forums.juniper.net/t5/SRX-Services-Gateway/Only-unit-0-is-valid-for-this-encapsulation-SRX/td-p/193969): Juniper Forum, June 2013
