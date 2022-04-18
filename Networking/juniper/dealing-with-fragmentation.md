# Dealing with Fragmentation

- [Dealing with Fragmentation](#dealing-with-fragmentation)
  - [Serious problem with fragmentation:](#serious-problem-with-fragmentation)
  - [Healthy fragmentation:](#healthy-fragmentation)
  - [Viewing the CPU over the last 60 seconds:](#viewing-the-cpu-over-the-last-60-seconds)
  - [References](#references)

## Serious problem with fragmentation: 
```bash
cm@fw# run show security flow statistics
Nov 18 13:05:30
    Current sessions: 60
    Packets forwarded: 177382053303
    Packets dropped: 38921220
    Fragment packets: 1878976296
    Pre fragments generated: 0
    Post fragments generated: 77078032
```

##  Healthy fragmentation: 
```bash
cm@fw> show security flow statistics
Nov 19 02:34:30
    Current sessions: 70
    Packets forwarded: 5294232719
    Packets dropped: 140881
    Fragment packets: 124
    Pre fragments generated: 0
    Post fragments generated: 0
```

## Viewing the CPU over the last 60 seconds: 
(in this example it's running at 99% all the time because of the massive fragmentation)
```bash
cm@fw# run show security monitoring performance spu
fpc  0  pic  0
Last 60 seconds:
 0:  99   1:  99   2:  99   3:  99   4:  99   5:  99
 6:  99   7:  99   8:  99   9:  99  10:  99  11:  99
12:  99  13:  99  14:  99  15:  99  16:  99  17:  99
18:  99  19:  99  20:  99  21:  99  22:  99  23:  99
24:  99  25:  99  26:  99  27:  99  28:  99  29:  99
30:  99  31:  99  32:  99  33:  99  34:  99  35:  99
36:  99  37:  99  38:  99  39:  99  40:  99  41:  99
42:  99  43:  99  44:  99  45:  99  46:  99  47:  99
48:  99  49:  99  50:  99  51:  99  52:  99  53:  99
54:  99  55:  99  56:  99  57:  99  58:  99  59:  99
```

or you can look at the cpu at this moment, but also some other stats: 
```bash
cm@fw# run show security monitoring fpc 0
FPC 0
  PIC 0
    CPU utilization          :   36 %
    Memory utilization       :   51 %
    Current flow session     :  109
    Current flow session IPv4:  109
    Current flow session IPv6:    0
    Max flow session         : 262144
Total Session Creation Per Second (for last 96 seconds on average):    4
IPv4  Session Creation Per Second (for last 96 seconds on average):    4
IPv6  Session Creation Per Second (for last 96 seconds on average):    0
```



## References
- [[SRX] Behavior of the 'df-bit' flag when IPSec is encapsulated](https://kb.juniper.net/InfoCenter/index?page=content&id=KB25625):  
