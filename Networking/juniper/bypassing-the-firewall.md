# bypassing the firewall


You can create a filter, and anything it captures can be sent to "packet-mode" to bypass the firewall filters. 
When this is enabled, all matching flows bypass any firewall settings in the "security" section.  (The bad news is that this also includes the NAT statements, so you will have problems getting traffic out of the firewall with this enabled.)

Capture and filter all traffic in one direction: 
```
set firewall family inet filter bypass_flowd term t1 from source-address 54.71.60.237/32
set firewall family inet filter bypass_flowd term t1 from destination-address 192.168.1.147/32
set firewall family inet filter bypass_flowd term t1 then count c1
set firewall family inet filter bypass_flowd term t1 then packet-mode
```

Capture and filter all traffic in the other direction: 
```
set firewall family inet filter bypass_flowd term t2 from source-address 192.168.1.147/32
set firewall family inet filter bypass_flowd term t2 from destination-address 54.71.60.237/32
set firewall family inet filter bypass_flowd term t2 then count c2
```

And make sure that you allow all other traffic, so the rest doesn't get blocked.  
```
set firewall family inet filter bypass_flowd term t2 then packet-mode
set firewall family inet filter bypass_flowd term t3 then accept
```



