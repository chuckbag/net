# Failover Configuration

How to get the ASA to work with the redundant unit.  

## Defining Secondary IPs for Redundant Firewall: 
It is not required for interfaces to have secondary IPs (IPs for the second firewall).  In this example, the secondary firewall has IPs for the g0/0 and g0/1 interface, but not the g0/3 interface.

```
int Gi0/0
ip address 32.41.23.14 255.255.255.248 standby 32.41.23.13
int Gi0/
ip address 69.39.78.254 255.255.255.248 standby 69.39.78.253
int Gi0/3
ip address 10.33.16.1 255.255.255.240
!
```

## Failover Settings for Primary Firewall: 
Here we enable fail-over, define the g0/2 interface as the fail-over link, define the secret key for both firewalls to talk with, and define heartbeat IPs for the interface

```
conf t
!
failover
failover lan unit primary
failover lan interface hearbeat Gi0/2
failover interface-policy 25%
failover key hex eecda584199414f30777504666ef392
failover link heartbeat gi0/2
failover interface ip heartbeat 10.33.8.1 255.255.255.240 10.33.8.2
!
end
```

## Monitor Settings (for Primary Firewall): 
You should monitor your interfaces, so if there is a failure on that interface, the firewall will fail over.  (Make sure that logic works for your specific deployment.)  If so, then define each interface name to be monitored: 

```
monitor-interface dmz1
monitor-interface dmz2
monitor-interface serverlab
monitor-interface desktops
```

## Failover Settings for the Secondary Firewall
There's not a lot of configs that you need to enter on the secondary firewall.  It will suck down all the configs from the primary once you link them.  To do this, define what interface should be used, and it's IP.  Then enable the interface, define the firewall as the secondary, and give it the shared password.  

```
failover lan interface heartbeat gi0/2
failover interface ip heartbeat 10.33.8.1 255.255.255.240 standby 10.50.33.2
!
int gi0/3
no shut
exit
!
failover lan unit secondary
failover key hex eecda584199414f30777504666ef392f
failover
```

## Viewing Failover Status: 
This is the view from the secondary firewall before it's copied the configs from the primary: 

```
ciscoasa# sh fail
Failover On
Failover unit Secondary
Failover LAN Interface: heartbeat GigabitEthernet0/2 (down)
Unit Poll frequency 1 seconds, holdtime 15 seconds
Interface Poll frequency 5 seconds, holdtime 25 seconds
Interface Policy 25%
Monitored Interfaces 0 of 160 maximum
Version: Ours 8.4(5), Mate Unknown
Last Failover at: 16:47:36 UTC Apr 11 2014
        This host: Secondary - Negotiation
                Active time: 0 (sec)
                slot 0: ASA5520 hw/sw rev (2.0/8.4(5)) status (Up Sys)
                slot 1: empty
        Other host: Primary - Not Detected
                Active time: 0 (sec)
                slot 0: empty
                slot 1: empty
Stateful Failover Logical Update Statistics
        Link : heartbeat GigabitEthernet0/2 (down)
        Stateful Obj    xmit       xerr       rcv        rerr
        General         0          0          0          0
        sys cmd         0          0          0          0
        up time         0          0          0          0
        RPC services    0          0          0          0
        TCP conn        0          0          0          0
        UDP conn        0          0          0          0
        ARP tbl         0          0          0          0
        Xlate_Timeout   0          0          0          0
        IPv6 ND tbl     0          0          0          0
        VPN IKEv1 SA    0          0          0          0
        VPN IKEv1 P2    0          0          0          0
        VPN IKEv2 SA    0          0          0          0
        VPN IKEv2 P2    0          0          0          0
        VPN CTCP upd    0          0          0          0
        VPN SDI upd     0          0          0          0
        VPN DHCP upd    0          0          0          0
        SIP Session     0          0          0          0
        Route Session   0          0          0          0
        User-Identity   0          0          0          0
        Logical Update Queue Information
                        Cur     Max     Total
        Recv Q:         0       0       0
        Xmit Q:         0       0       0
ciscoasa#
```

## References: 
- [CLI Book 1](https://www.cisco.com/c/en/us/td/docs/security/asa/asa91/configuration/general/asa_91_general_config/ha_failover.html#98842): Cisco ASA Series General Operations CLI Configuration Guide, 9.1