# @#$%@! enable passwords

## Overview: 
The following are a list of steps for the different devices out there, and how to clear their passwords

## c3600
press `BREAK` within 60 seconds of power up, then enter the following at the rommon prompt. 

```
rommon 1> confreg 0x2142
rommon 2> reset
```

the router will then reboot with a clean os (ignoring the current saved config).  

At this point you can either blast the saved config with the wr erase command, or modify the password on the saved config with the following commands (at the enable prompt): 
```
Router# config mem
Router# conf t
Router(config)# enable secret <password>
```

At this point, you will need to re-enable all the interfaces, (no-shut them).  So repeat the following for each int that should be up.
```
Router(config)# int e0/0
Router(config)# no shut
```

Then enable the saved config and save
```
Router(config)# config-register 0x2102
Router(config)# end
Router# wr mem
```

If you have access to the device, and simply want to reset it to its factory defaults, use the write erase command: 
```
tmp-swc01#write erase
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
*Mar  1 00:05:22.748: %SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
tmp-swc01#reload
Proceed with reload? [confirm]
*Mar  1 00:05:28.150: %SYS-5-RELOAD: Reload requested by admin on console. Reload reason: Reload command
```

 