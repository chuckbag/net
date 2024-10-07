# SNMP

## SNMP v.2c
```
snmp-server enable 
snmp-server community 0 ZFzBymAfuuVV  
snmp-server host serverlab 10.33.32.31 poll community 0 ZFzBymAfuuVV version 2c
```

## SNMP v.3
You need to define a user, a group associated to that user, and the hosts that are allowed to poll the firewall.  You also need to enable the snmp-server when your done.  

```
snmp-server group prtg_monitoring v3 priv
snmp-server user prtg prtg_monitoring v3 auth md5 pass123 priv des word123
snmp-server host inside 10.113.32.161 poll version 3 prtg
snmp-server enable
```

Where 
- `10.133.32.161` is the device allowed to poll the firewall
- `pass123` is the auth password
- `word123` is the encryption password
- `priv` means that we will both encrypt the auth and the data being sent back and forth

and you can test with the following unix command: 
```
snmpwalk -v3 -u prtg -A pass123 -l authPriv -a MD5 -x DES -X word123 172.16.0.10 | more
```

where 
- `172.16.0.16` is the IP of the firewall (assuming that you are polling from 10.133.32.161, otherwise your traffic will be filtered.  

## References: 
- [Cisco Security Appliance Command Line Configuration Guide, Version 8.0](http://www.cisco.com/en/US/docs/security/asa/asa80/configuration/guide/monitor.html)
- [Cisco ASA 5500 Series Configuration Guide using the CLI, 8.4 and 8.6](http://www.cisco.com/en/US/docs/security/asa/asa84/configuration/guide/monitor_snmp.html#wp1350063)
- [Cisco ASA 5500 Series Configuration Guide using the CLI, 8.2: Cisco](http://www.cisco.com/c/en/us/td/docs/security/asa/asa82/configuration/guide/config/monitor_snmp.html)
