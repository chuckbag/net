# snmp

## SNMP GET

### Basic Poll:
```bash
snmpget -v <version> -c <password> <hostname> <oid>
```

#### What is the hostname:
```bash
$ snmpget -v 2c -c I5W04rWq7g sl-csw02 .1.3.6.1.2.1.1.5.0
SNMPv2-MIB::sysName.0 = STRING: sl-csw02.europe.chuck.com
```

Basic Device Info:
```bash
$ snmpwalk -v 2c -c I5W05rWq7g -On sl-asw02 .1.3.6.1.2.1.1
.1.3.6.1.2.1.1.1.0 = STRING: Cisco IOS Software, Catalyst 4500 L3 Switch Software (cat4500e-IPBASEK9-M), Version 12.2(50)SG, RELEASE SOFTWARE (fc4)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2008 by Cisco Systems, Inc.
Compiled Wed 24-Dec-08 21:04 by pr
.1.3.6.1.2.1.1.2.0 = OID: .1.3.6.1.4.1.9.1.875
.1.3.6.1.2.1.1.3.0 = Timeticks: (751624544) 86 days, 23:50:45.44
.1.3.6.1.2.1.1.4.0 = STRING:
.1.3.6.1.2.1.1.5.0 = STRING: sl-asw02.europe.chuck.com
.1.3.6.1.2.1.1.6.0 = STRING:
.1.3.6.1.2.1.1.7.0 = INTEGER: 6
.1.3.6.1.2.1.1.8.0 = Timeticks: (0) 0:00:00.00
```

## SNMP WALK
View ARP Cache:
```bash
$ snmpwalk -v 2c -c I5105DWq7g sl-csw02 1.3.6.1.2.1.4.22.1.2
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.140 = STRING: 0:50:56:bd:74:6b
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.141 = STRING: 0:50:56:bd:6c:df
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.142 = STRING: 0:50:56:bd:61:dd
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.143 = STRING: 0:50:56:bd:7e:35
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.144 = STRING: 0:50:56:bd:d:be
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.145 = STRING: 0:50:56:bd:4e:c3
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.146 = STRING: 0:50:56:bd:58:46
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.147 = STRING: 0:50:56:bd:13:7d
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.148 = STRING: 0:18:8b:8a:b9:a7
IP-MIB::ipNetToMediaPhysAddress.920.10.218.198.149 = STRING: 0:1e:c9:e6:90:89
```


## References:
- [Cisco SNMP Wiki](http://docwiki.cisco.com/wiki/Simple_Network_Management_Protocol):
- [mibDepot](http://www.mibdepot.com/index.shtml): Repository of many companies SNMP MIB's