# snmp

## Make the changes: 

```Shell
snmp-server view all iso included
snmp-server group monitor v3 priv read all
snmp-server user prtg monitor v3 auth md5 loginPassword priv des encryptionPassword
```

## Check the settings: 

```Shell
nee-swc1c01#sh snmp user

User name      : prtg
Security model : v3
Engine ID      : f5717f28993a27ec6900
Authentication : MD5
Privacy        : DES
Group          : monitor
```

```Shell
nee-swc1c01#sh snmp group

Group name     : monitor
Security model : v3 priv
Read view      : all
Write view     : <no write view specified>
Notify view    : <no notify view specified>
nee-swc1c01#
```

```Shell
nee-swc1c01#sh snmp view
all iso - included
nee-swc1c01#
```

## Test the changes: 

```Shell
[chuck ~]$ snmpwalk -v3 -u prtg -A loginPassword -l authPriv -a MD5 -x DES -X encryptPassword 10.153.255.6 | more
SNMPv2-MIB::sysDescr.0 = STRING: Arista Networks EOS version 4.18.0F running on an Arista Networks DCS-7160-48YC6
SNMPv2-MIB::sysObjectID.0 = OID: SNMPv2-SMI::enterprises.30065.1.3011.7160.48.1654.6
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (96111329) 11 days, 2:58:33.29
SNMPv2-MIB::sysContact.0 = STRING:
SNMPv2-MIB::sysName.0 = STRING: nee-swc1c04
SNMPv2-MIB::sysLocation.0 = STRING:
SNMPv2-MIB::sysServices.0 = INTEGER: 14
SNMPv2-MIB::sysORLastChange.0 = Timeticks: (61516313) 7 days, 2:52:43.13
SNMPv2-MIB::sysORID.1 = OID: TCP-MIB::tcpMIB
SNMPv2-MIB::sysORID.2 = OID: UDP-MIB::udpMIB
SNMPv2-MIB::sysORID.3 = OID: SNMPv2-MIB::snmpMIB
SNMPv2-MIB::sysORID.4 = OID: SNMP-VIEW-BASED-ACM-MIB::vacmBasicGroup
```
