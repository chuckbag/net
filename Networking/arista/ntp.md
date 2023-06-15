# NTP

## setting up ntp 

```
ntp server 10.113.32.248
```


## Checking the status

```
nee-swc1c01#show ntp associations
     remote          refid      st t when  poll reach   delay   offset  jitter
==============================================================================
*10.113.32.248   172.16.0.248    4 u   20    64  37      1.82    3.799   5.763
```

```
nee-swc1c01#sh ntp status
unsynchronised
   polling server every 64 s
```


## References: 
- https://eos.arista.com/forum/ntp-serve-on-loopbacks-or-ip-address-virtual-interfaces/