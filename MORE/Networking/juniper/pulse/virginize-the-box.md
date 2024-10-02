# Virginize the box

## Overview:
If you get a Juniper MAG 2600 and you need to completely clean it of all it's configs and settings, you can bring it back to its factory defaults with the following steps:

## Connect Via Console:
You need to connect to the device via a console connection, and then reboot the system. 

## Reset to Factory Settings:
From the RS232 console, go to the System Operations (option 4) :
```
:Press Enter to modify system settings.

Welcome to the Juniper Networks Junos Pulse Secure Access Service Serial Console!

Current version: 7.2R2 (build 21017)
Rollback version: 7.1R1 (build 17675)
Reset version:  4.1R1 (build 17057)

Licensing Hardware ID: YdTV5a6JH5mu6x8VQ
Serial Number: 6411767226620186

Please choose from among the following options:
   1. Network Settings and Tools
   2. Create admin username and password
   3. Display log/status
   4. System Operations
   5. Toggle password protection for the console (Off)
   6. Create a Super Admin session.
   7. System Maintenance
   8. Reset allowed encryption strength for SSL
Choice: 4
```

Then select Factory reset this Junos Pulse Secure Access Service (option 5)  to clear all the current configs out:
```
Please choose the operation to perform:
   1. Reboot this Junos Pulse Secure Access Service
   2. Shutdown this Junos Pulse Secure Access Service
   3. Restart services at this Junos Pulse Secure Access Service
   4. Rollback this Junos Pulse Secure Access Service
   5. Factory reset this Junos Pulse Secure Access Service
   6. Clear all configuration data at this Junos Pulse Secure Access Service
 <return to go back to main menu>
Choice: 4
```