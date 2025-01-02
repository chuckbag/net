# upgrading the os

## Uploading the BIN file: 
copy the file from the server (10.33.195.78) to the device (flash:/)

```
copy tftp://10.33.195.78/c3750-ipservicesk9-mz.150-2.SE2.bin flash:/
```

Then set the new image as the one you want to boot from: 

```
conf t
boot system switch all flash:/c3750-ipservicesk9-mz.150-2.SE2.bin
end
```

Finally, save your changes and reboot to the new os. 

```
wr mem
reload
```

## References: 
- [How To Copy a System Image from One Device to Another](http://www.cisco.com/en/US/products/hw/routers/ps233/products_tech_note09186a00800a6744.shtml)
- [Software Upgrade Procedure](http://www.cisco.com/en/US/products/ps5855/products_tech_note09186a00801fc986.shtml)
- [Catalyst 3750 Software Upgrade in a Stack Configuration with Use of the Command-Line Interface](http://www.cisco.com/en/US/products/hw/switches/ps5023/products_configuration_example09186a00804799d7.shtml)