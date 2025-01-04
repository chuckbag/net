# creating vlans and adding to interfaces

Define the specific vlans: 
```
int vlan 12
 des What_this_vlan_is
 ip address 10.15.12.30/24
 exit
```

tag them on switch ports: 
```
int range vlan 1-1001
 tag ten1/1 - 1/30
 no shut
 exit
```

Resulting Configs: 
```
!
interface Vlan 12
 description What_this_vlan_is
 ip address 10.15.12.30/24
 tagged TenGigabitEthernet 1/1-1/30
 tagged Port-channel 48
 no shutdown
!
```

