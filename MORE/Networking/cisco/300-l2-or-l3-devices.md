# L2 or L3 devices
You can configure the device to either be a crummy layer 3 device (that does not do dhcp or hsrp/vrrp) or a layer 2 device.  When you make the change, you need to reboot the box, and all the prior configs are blown away.  you can do this with one of the following commands:

```
#set system mode router
#set system mode switch
```
