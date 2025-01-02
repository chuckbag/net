# DHCP Server

Setting up a dhcp server on a router or layer3 switch.  

Create the DHCP Pool: 

```
ip dhcp pool dhcp-voip-pbx
   network 10.33.128.0 255.255.255.0
   domain-name cmed.us
   dns-server 8.8.8.8 4.4.4.4
   default-router 10.33.128.1
   lease infinite
!
```

define IPs that should not be given out in the DHCP leases: 
```
ip dhcp excluded-address 10.33.128.1 10.33.128.31
```

enable the dhcp service (it's enabled by default)
```
service dhcp
```

## References: 
- [Configuring DHCP](http://www.cisco.com/en/US/docs/ios/12_2/ip/configuration/guide/1cfdhcp.html) (ios 12.2)