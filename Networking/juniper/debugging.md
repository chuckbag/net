# debugging


## security match-policies
You can use the security match-policies command to test the flow from one device to the other.  

This is an example of a test that is correct: 
```bash
> show security match-policies from-zone b1-trans2 to-zone n1-srv2 source-ip 10.50.81.39 destination-ip 10.120.81.39 protocol tcp source-port 12345 destination-port 443

node0:
--------------------------------------------------------------------------
Policy: rule1, action-type: permit, State: enabled, Index: 5
0
  Policy Type: Configured
  Sequence number: 1
  From zone: b1-trans2, To zone: n1-srv2
  Source addresses:
    any-ipv4(global): 0.0.0.0/0
    any-ipv6(global): ::/0
  Destination addresses:
    any-ipv4(global): 0.0.0.0/0
    any-ipv6(global): ::/0
  Application: any
    IP protocol: 0, ALG: 0, Inactivity timeout: 0
      Source port range: [0-0]
      Destination port range: [0-0]
  Per policy TCP Options: SYN check: Yes, SEQ check: No

{primary:node0}
```

And this is an example of a test that fails: 
```bash
> show security match-policies from-zone b1-trans2 to-zone n1-srv2 source-ip 10.50.81.39 destination-ip 10.120.81.39 protocol tcp source-port 12345 destination-port 443
node0:
--------------------------------------------------------------------------
Policy: Default-Policy, action-type: deny-all, State: enabled, Index: 2
  Sequence number: 2

{primary:node0}
```

## Firewall Filter

track traffic going through the firewall.  
```
edit
set firewall family inet filter TESTCOUNT term MATCH from source-address 96.230.36.204/32
set firewall family inet filter TESTCOUNT term MATCH from destination-address 38.111.225.242/32
set firewall family inet filter TESTCOUNT term MATCH from protocol tcp
set firewall family inet filter TESTCOUNT term MATCH then count testcounter
set firewall family inet filter TESTCOUNT term MATCH then accept
set firewall family inet filter TESTCOUNT term EVERYTHINGELSE then accept
commit confirmed 10
```

```bash
# run show firewall filter TESTCOUNT

Filter: TESTCOUNT
Counters:
Name                                                Bytes              Packets
testcounter                                             0                    0
```


## References: 
- [show security match-policies](http://www.juniper.net/techpubs/en_US/junos12.1/topics/reference/command-summary/show-security-match-policies.html): Junos 12.1 Oct 2012
