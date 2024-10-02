# QoS over VPNs

firewall settings on the 10.36.0.0/16 local side: 
```
set firewall family inet filter block-lmit term 1 from source-address 10.36.0.0/16
set firewall family inet filter block-lmit term 1 from destination-address 10.33.0.0/16
set firewall family inet filter block-lmit term 1 then policer lmit-vpn-traffic
set firewall family inet filter block-lmit term 1 then accept

set firewall family inet filter block-lmit term 2 then accept

set firewall policer lmit-vpn-traffic if-exceeding bandwidth-limit 175m
set firewall policer lmit-vpn-traffic if-exceeding burst-size-limit 200k
set firewall policer lmit-vpn-traffic then discard

commit check
commit confirmed 5
commit comment "fixing vpn-qos"
```

firewall settings on the 10.33.0.0/16 local side: 
```
set firewall family inet filter block-lmit term 1 from source-address 10.33.0.0/16
set firewall family inet filter block-lmit term 1 from destination-address 10.36.0.0/16
set firewall family inet filter block-lmit term 1 then policer lmit-vpn-traffic
set firewall family inet filter block-lmit term 1 then accept

set firewall family inet filter block-lmit term 2 then accept

set firewall policer lmit-vpn-traffic if-exceeding bandwidth-limit 175m
set firewall policer lmit-vpn-traffic if-exceeding burst-size-limit 200k
set firewall policer lmit-vpn-traffic then discard

commit check
commit confirmed 5
commit comment "fixing vpn-qos"
```

