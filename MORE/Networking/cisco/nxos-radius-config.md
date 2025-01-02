# Nexus Radius Config

Note that the following configs are not in their correct order, but I mixed them up to better discuss them. 

```
radius-server host 10.218.73.8 key 7 "x99gL3e1ow" authentication accounting
radius-server host 10.218.73.11 key 7 "x99gL3e1ow" authentication accounting
radius-server directed-request
aaa authentication login console local
aaa authentication login default group ACS
aaa group server radius ACS
    server 10.218.73.8
    server 10.218.73.11
    use-vrf management
```

Reviewing the configs:

- First two lines (radius-server host) define what the IP of the radius server is, the preshared key (in quotes), the encryption of the key (7= encrypted, 0= plaintext), and that accounting should be enabled.
- The third line (radius-server directed-request) states that log in requests should be sent to the radius server.  
- The forth line (aaa authentication login console) specifies that console logins should be authenticated with the local accounts, and not against the radius server
- The fifth line (aaa authentication login default) specifies that default log in's should be authenticated as per the instructions in the group "ACS".  See next line for the description of that group.  
- The sixth line (aaa group server radius) defines the radius login group "ACS".  It lists the two servers to use, and it also states to use the vrf named "management" for all aaa comms.


