# normal switchport configs

## Config Changes: 
define the specifics for each interface: 
```
conf t
interface TenGigabitEthernet 1/2
 description This_host_and_interface
end
```

define configs for a group of switches: 
```
int range ten1/1-30
 no ip address
 portmode hyb
 switchport
 flowcontrol rx on tx off
 no shut
 exit
```

## Results: 
Example interface config:
```
!
interface TenGigabitEthernet 1/2
 description This_host_and_interface
 no ip address
 portmode hybrid
 switchport
 flowcontrol rx on tx off 
 no shutdown
!
```

