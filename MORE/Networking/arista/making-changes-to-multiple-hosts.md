# making changes to multiple hosts

```Shell
nee-swc1c03(config-if-Et1)#int e1-6
nee-swc1c03(config-if-Et1-6)#no shut
nee-swc1c03(config-if-Et1-6)#sh active
interface Ethernet1
   description bos-ntxvdi001
   switchport trunk allowed vlan 10,12,15-20,22,33,40,50-51,65
   switchport mode trunk
interface Ethernet2
   description bos-ntxvdi002
   switchport trunk allowed vlan 10,12,15-20,22,33,40,50-51,65
   switchport mode trunk
interface Ethernet3
   description bos-ntxvdi003
   switchport trunk allowed vlan 10,12,15-20,22,33,40,50-51,65
   switchport mode trunk
interface Ethernet4
   description bos-ntxvdi004
   switchport trunk allowed vlan 10,12,15-20,22,33,40,50-51,65
   switchport mode trunk
interface Ethernet5
   description nee-ntxprd010
   switchport trunk allowed vlan 10,12,15-20,22,33,40,50-51,65
   switchport mode trunk
interface Ethernet6
   description nee-ntxprd009
   switchport trunk allowed vlan 10,12,15-20,22,33,40,50-51,65
   switchport mode trunk
nee-swc1c03(config-if-Et1-6)#
```
