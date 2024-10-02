# snmp

Enable the snmp services, and the hosts characteristic. 
```
set snmp description "fw1m01 - management firewall "
set snmp location n1.fra
set snmp contact "root@cmed.us"
set snmp community badsnmppassword authorization read-only
```

Allow access to the router from the snmp sources
```
! traffic could come from one of the two interfaces
set security zones security-zone untrust interfaces fe-0/0/2.802 host-inbound-traffic system-services snmp
set security zones security-zone trust   interfaces fe-0/0/0.952 host-inbound-traffic system-services snmp
! the source of the traffic could come from the two class C networks
set snmp community badsnmppassword clients 10.120.34.0/24
set snmp community badsnmppassword clients 10.50.32.0/24
```

## References: 
- [SRX Getting Started - Configure SNMP Agent](http://kb.juniper.net/InfoCenter/index?page=content&id=KB16545&smlogin=true): kb16545, Mar 5, 2013
