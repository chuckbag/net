# ASA 5000 Password Reset

```
Use BREAK or ESC to interrupt boot.
Use SPACE to begin boot immediately.
Boot interrupted.                              
Use ? for help.
rommon #0> confreg
Current Configuration Register: 0x00000001
Configuration Summary:
  boot default image from Flash
Do you wish to change this configuration? y/n [n]: n
rommon #1> confreg 0x41
Update Config Register (0x41) in NVRAM...
rommon #2> boot
Launching BootLoader...
Boot configuration file contains 1 entry.
Loading disk0:/ASA_7.0.bin... Booting...
###################
...
Ignoring startup configuration as instructed by configuration register.
Type help or '?' for a list of available commands.
hostname> enable
Password:
hostname# configure terminal
hostname(config)# copy startup-config running-config
Destination filename [running-config]?
Cryptochecksum(unchanged): 7708b94c e0e3f0d5 c94dde05 594fbee9
892 bytes copied in 6.300 secs (148 bytes/sec)
hostname(config)# enable password NewPassword
hostname(config)# config-register 0x1
hostname(config)# reload
```

## References: 
- [Monitoring and Troubleshooting](http://www.cisco.com/en/US/docs/security/asa/asa71/configuration/guide/trouble.html#wp1058131): Security Appliance Command Line Config Guide, v7.1