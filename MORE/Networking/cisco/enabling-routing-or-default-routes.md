# Enabling routing or default routes

if the switch is not forwarding packets (not in router mode)

```
ip default-gateway 1.1.1.1
```

If the switch should forward packets (should become a router)

```
! enable routing
ip routing
! create two interfaces: 
int vlan 10
 ip address 1.2.3.2 255.255.255.0
 exit
int vlan 11
 ip address 1.2.4.2 255.255.255.0
 exit
! set a default route
ip route 0.0.0.0 0.0.0.0 1.2.3.1
```

## Ref: 
- [Assigning the Switch IP Address and Default Gateway](http://www.cisco.com/en/US/docs/switches/lan/catalyst3750x_3560x/software/release/12.2_55_se/configuration/guide/swipaddr.html#wp1037806): Catalyst 3750-X and 3560-X Switch Software Configuration Guide, Release 12.2(55)SE

