# Network Reset Cause

If you run the command twice while failing traffic is occurring, you will see the counters of the failed traffic step up, and give you a clue to what is wrong with your settings. 

```
[dude@lb1n01:Active:In Sync] ~ # tmsh show net rst-cause
--------------------------------
TCP/IP Reset Cause
RST Cause:                 Count
--------------------------------
Connection limit exceeded     76
Flow expired (sweeper)         1
HA disconnect                 16
No local listener             12
No pool member available      72
No route to host              33
No server selected           377
TCP retransmit timeout        29
[dude@lb1n01:Active:In Sync] ~ #
```

```
[dude@lb1n01:Active:In Sync] ~ # tmsh show net rst-cause
--------------------------------
TCP/IP Reset Cause
RST Cause:                 Count
--------------------------------
Connection limit exceeded     76
Flow expired (sweeper)         1
HA disconnect                 16
No local listener             12
No pool member available      85
No route to host              33
No server selected           377
TCP retransmit timeout        29
```
