# ntp

## Make change
```
edit 

set system ntp server 10.36.35.32 version 4 
set system ntp server 10.33.35.32 version 4 prefer

show | compare | no-more
commit check


commit comment "  add ntp" and-quit
show configuration | no-more
```

## Troubleshoot
Confirm stratum 
```bash
fw> show ntp status
status=0664 leap_none, sync_ntp, 6 events, event_peer/strat_chg,
version="ntpd 4.2.0-a Tue Dec 13 15:04:13  2016 (1)",
processor="octeon", system="JUNOS15.1X49-D70.3", leap=00, stratum=3,
precision=-18, rootdelay=17.257, rootdispersion=7.170, peer=11237,
refid=10.33.35.32,
reftime=df1484b1.63d91316  Tue, Aug  7 2018 20:50:57.390, poll=6,
clock=df1484b2.81901dfc  Tue, Aug  7 2018 20:50:58.506, state=3,
offset=0.000, frequency=0.000, jitter=9.560, stability=0.000

fw>
```

Check the neighbors
```bash
fw> show ntp associations
   remote         refid           st t when poll reach   delay   offset  jitter
===============================================================================
 10.36.35.32      .STEP.          16 -    - 1024    0    0.000    0.000 4000.00
*10.33.35.32      129.6.15.30      2 -   25   64  177    1.297    2.951   8.563

fw>
```

## References
- [Getting Started - Configure Time and NTP Client](https://kb.juniper.net/InfoCenter/index?page=content&id=kb15756): Juniper KB 15756, Dec 2017
