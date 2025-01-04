# port channel switchport configs

enable trunking and lldp for port channeling.  
```
service-class dynamic dot1p
!
protocol lldp
  advertise management-tlv system-capabilities system-description system-name 
```

Port Channel def
```
interface Port-channel 48
 description link to core
 no ip address
 switchport
 vlt-peer-lag port-channel 48
 no shutdown
!
```

Interface linked to port channel
```
interface TenGigabitEthernet 1/48
 description uplink (swc1n02:e1 /40)
 no ip address
!  
 port-channel-protocol LACP 
  port-channel 48 mode active 
 no shutdown
!
```

