# Virginising the device


There are two ways to clear the configs on a JunOS box.  

## factory-default
the simplest way to clear the configs on a JunOS box, is to use the load factory-default command.  This replaces the current configs with the original ones that were shipped with the device.  
```bash
root@fw# load factory-default
warning: activating factory configuration

[edit]
root@fw# 
```

You will need to [reset the root password](password-reset.md) before you can commit the changes though.  

## zeroize
If you need to clear everything from the box including all log files and the like, then you should use the zeroize command.  (This is good if equipment is leaving your ownership.)
```bash
root@fw> request system zeroize
warning: System will be rebooted and may not boot without configuration
Erase all data, including configuration and log files? [yes,no] (no) yes

warning: zeroizing re0

root@fw> 
Rebooting...
```

Note that the format of this command might be different depending on the FIPS status of your system.  You can confirm the correct context with the command: 
```bash
root@fw> help apropos zeroize
request system zeroize
    Erase all data, including configuration and log files
```

## Save changes and restart: 
Make sure the changes you entered were good, then view them, and then save
```bash
commit check
show | compare
commit and-quit
```

Then restart the device: 
```bash
root@fw> request system reboot 
```

## SRX Default user and password: 
The default user is root, and at first, you do not need to enter a password
```bash
Amnesiac (ttyu0)

login: root

--- JUNOS 12.1X44-D40.2 built 2014-08-28 12:20:14 UTC
root@%   
```

## References: 
- [[SRX] Getting Started - Factory Reset](https://supportportal.juniper.net/s/article/SRX-Getting-Started-Factory-Reset?language=en_US): Using the factory reset button on select SRX firewalls. 
- [Rebooting the Device with the CLI](https://www.juniper.net/techpubs/software/junos-es/junos-es92/junos-es-admin-guide/rebooting-the-device-with-the-cli.html): 