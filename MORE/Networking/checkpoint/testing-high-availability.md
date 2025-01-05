# Testing High Availability

## Confirm HA Status: 
use the cphaprobe command to make sure that you have an active/standby pair, and what the local IP's are between them.  (normally these IPs are part of the cluster link.)

```
[Expert@BOS-CKP-FW2:0]# cphaprob stat

Cluster Mode:   High Availability (Active Up) with IGMP Membership
Number     Unique Address  Assigned Load   State
1          169.254.0.1     100%            Active
2 (local)  169.254.0.2     0%              Standby
[Expert@BOS-CKP-FW2:0]#
```

Check the number of connections on both firewalls.  This shows that they are properly syncing state between them.  (the numbers will not match up exactly, but should be close.)
```
[Expert@BOS-CKP-FW2:0]# fw tab -t connections -s

HOST                  NAME                                ID #VALS #PEAK #SLINKS
localhost             connections                       8158  1508 14569    5168
[Expert@BOS-CKP-FW2:0]#
```

## Failover 

```
clusterXL_admin down
```

```
cluserXL_admin up 
```
