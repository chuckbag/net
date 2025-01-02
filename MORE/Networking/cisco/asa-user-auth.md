# User Auth

## Notes: 

Review the aaa configs: 

```
LEX-ASA-FW1/act# more system:running-config | beg aaa-server
aaa-server ImpAAA protocol ldap
aaa-server ImpAAA (inside) host 172.1.0.29
 server-type auto-detect
aaa-server NPS_Inside protocol radius
aaa-server NPS_Inside (inside) host 172.1.0.29
 key 123abc123abc
aaa-server IMP-NETWORK-AAA protocol radius
aaa-server IMP-NETWORK-AAA (inside) host 10.1.3.28
 timeout 7
 key 123abc123abc
aaa-server IMP-NETWORK-AAA (inside) host 10.1.3.29
 timeout 7
 key 123abc123abc
aaa-server 2FactorPilot protocol radius
 max-failed-attempts 5
aaa-server 2FactorPilot (inside) host 10.1.2.2
 timeout 60
 key 123abc123abc
 authentication-port 1812
 accounting-port 1813
aaa-server NPS_LXI protocol radius
aaa-server NPS_LXI (inside) host 10.1.2.5
 key 123abc123abc
aaa-server Imp_NPS protocol radius
aaa-server Imp_NPS (inside) host 10.1.2.5
```

```
LEX-ASA-FW1/act# 
LEX-ASA-FW1/act# sh run aaa-server 
aaa-server ImpAAA protocol ldap
aaa-server ImpAAA (inside) host 172.1.0.29
 server-type auto-detect
aaa-server NPS_Inside protocol radius
aaa-server NPS_Inside (inside) host 172.1.0.29
 key *****
aaa-server IMP-NETWORK-AAA protocol radius
aaa-server IMP-NETWORK-AAA (inside) host 10.1.3.28
 timeout 7
 key *****
aaa-server IMP-NETWORK-AAA (inside) host 10.1.3.29
 timeout 7
 key *****
aaa-server 2FactorPilot protocol radius
 max-failed-attempts 5
aaa-server 2FactorPilot (inside) host 10.1.2.2
 timeout 60
 key *****
 authentication-port 1812
 accounting-port 1813
aaa-server NPS_LXI protocol radius
aaa-server NPS_LXI (inside) host 10.1.2.5
 key *****
aaa-server Imp_NPS protocol radius
aaa-server Imp_NPS (inside) host 10.1.2.5
 key *****    
 radius-common-pw *****
aaa-server Imp_NPS (inside) host 10.1.1.5
 key *****
 radius-common-pw *****
LEX-ASA-FW1/act# 
```

Test your auth services: 

```
LEX-ASA-FW1/act# test aaa authentication 2FactorPilot host 10.1.2.2
Username: cmercier
Password: *********
INFO: Attempting Authentication test to IP address <10.1.2.2> (timeout: 62 seconds)
ERROR: Authentication Rejected: AAA failure
LEX-ASA-FW1/act# 
```

## Ref: 