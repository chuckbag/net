# BGP

## Overview: 
This is an example cisco bgp config. 

For more details on BGP Routing theory, see [BGP Routing Theory](../Eth-IP/layer3/bgpv4.md).

## Configs: 

```
!
ip prefix-list as-6561-out description Announced AS6561 Routes
ip prefix-list as-6561-out seq 20 permit 16.109.104.0/24
ip prefix-list as-6561-out seq 30 permit 16.109.110.0/23
ip prefix-list as-6561-out seq 40 permit 16.109.106.0/24
ip prefix-list as-6561-out seq 50 permit 16.109.105.0/24
!
route-map set-community-to-gc permit 10
 match ip address prefix-list as-6561-out
 set community 3549:300
!
route-map set-local-pref-gc permit 10
 match as-path 1
 set local-preference 100
!
ip as-path access-list 1 permit .*
router bgp 6561
 no synchronization
 bgp log-neighbor-changes
 bgp dampening

 network 16.109.104.0
 network 16.109.105.0
 network 16.109.106.0
 network 16.109.110.0 mask 255.255.254.0

 neighbor 172.25.25.3 remote-as 16561
 neighbor 172.25.25.3 update-source Loopback25
 neighbor 172.25.25.3 version 4

 neighbor 208.48.23.245 remote-as 3549
 neighbor 208.48.23.245 password 7 071D29455F0E2E501E202D29
 neighbor 208.48.23.245 version 4
 neighbor 208.48.23.245 send-community
 neighbor 208.48.23.245 soft-reconfiguration inbound
 neighbor 208.48.23.245 route-map set-local-pref-gc in
 neighbor 208.48.23.245 route-map set-community-to-gc out

ip route 16.109.104.0 255.255.255.0 16.109.110.14
ip route 16.109.104.0 255.255.255.0 Null0 254
ip route 16.109.105.0 255.255.255.0 Null0 254
ip route 16.109.106.0 255.255.255.0 16.109.110.5
ip route 16.109.106.0 255.255.255.0 Null0 254
ip route 16.109.108.0 255.255.254.0 16.109.110.14
ip route 16.109.108.136 255.255.255.255 208.48.23.245
ip route 16.109.108.138 255.255.255.255 208.48.23.245
ip route 16.109.110.0 255.255.254.0 Null0 254
ip route 16.109.110.0 255.255.254.0 16.109.110.14
```