# Virginise the box

## Overview: 
How to erase all the configs on the WLC and start all over again with the factory default configs: 

## Steps: 
Power cycle the device
During the boot process press <ESC> when prompted: 
```
WLCNG Boot Loader Version 1.0.16 (Built on Feb 28 2011 at 13:14:54 by cisco)
Board Revision 0.0 (SN: PSZ164206R5, Type: AIR-CT2504-K9) (P)
Verifying boot loader integrity... OK.
OCTEON CN5230C-SCP pass 2.0, Core clock: 750 MHz, DDR clock: 330 MHz (660 Mhz data rate)
CPU Cores:  4
DRAM:  1024 MB
Flash: 32 MB
Clearing DRAM........ done
Network: octeth0', octeth1, octeth2, octeth3
  ' - Active interface
  E - Environment MAC address override
CF Bus 0 (IDE): OK
IDE device 0:
 - Model: 1GB CompactFlash Card Firm: CF B612J Ser#: A191105498A10j6CI4eq
 - Type: Hard Disk
 - Capacity: 977.4 MB = 0.9 GB (2001888 x 512)
Press <ESC> now to access the Boot Menu...
```

After you press the escape key, you will get prompted with the following message.  Select "4" to clear the current running configuration.  
```
============================================================
 Boot Loader Menu
============================================================
 1. Run primary image (7.3.112.0) - Active
 2. Run backup image (7.2.111.3)
 3. Change active boot image
 4. Clear configuration
 5. Format FLASH Drive
 6. Manually update images
------------------------------------------------------------
Enter selection: 4
```

Afterword, you will be promoted to enter in a new user/pass.