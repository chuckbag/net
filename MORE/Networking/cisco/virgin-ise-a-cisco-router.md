# Virgin-ise a Cisco Router


## Deleting the config files:  
There are two files you need to remove to clean out the configs of a device.  The first is the config file, and the second is the file that records all the vlan info.  

As long as you delete the following, the device will come up empty after reboot.  (Note that you need root on the device to do this.)

```
delete flash:/config.text
delete flash:/vlan.dat
reload
```

## Using Commands to delete configs: 

The task at hand is to clear all the configs from the router and bring it to it's factory settings. To do this you only need to do three things:

- erase startup-config, and accept
- reload, and accept
- after startup, deny running "initial configuration dialog"

And that's it. The box still has the same os version on it, but absolutely no configs.

Now you can write your own configs, without having to undo someone else's.

### Example From Console:
```
c2621-1#erase startup-config 
Erasing the nvram filesystem will remove all files! 
Continue? [confirm] y
[OK] 

Erase of nvram: complete  

c2621-1#reload 
Proceed with reload? [confirm]y 

00:12:54: %SYS-5-RELOAD: Reload requested System Bootstrap, Version 11.3(2)XA4, 
RELEASE SOFTWARE (fc1) Copyright (c) 1999 by cisco Systems, Inc. 
TAC:Home:SW:IOS:Specials for info 
C2600 platform with 49152 Kbytes of main memory  
program load complete, entry point: 0x80008000, size: 0x8ca18c 
Self decompressing the image : ################################################# 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
################################################################################ 
############ 
[OK]  

Smart Init is disabled. 
IOMEM set to: 25  
Using iomem percentage: 25                
Restricted Rights Legend  
Use, duplication, or disclosure by the Government is subject to restrictions as set forth 
in subparagraph (c) of the Commercial Computer Software - Restricted Rights clause at FAR 
sec. 52.227-19 and subparagraph (c) (1) (ii) of the Rights in Technical Data and Computer 
Software clause at DFARS sec. 252.227-7013.             
cisco Systems, Inc.            
170 West Tasman Drive            
San Jose, California 95134-1706    
Cisco Internetwork Operating System Software IOS (tm) 
C2600 Software (C2600-IS-M), Version 12.1(3)XI, EARLY DEPLOYMENT RELEASE SOFTWARE (fc1) 
TAC:Home:SW:IOS:Specials for info Copyright (c) 1986-2000 by cisco Systems, Inc.
Compiled Sat 29-Jul-00 07:57 by kirthi 
Image text-base: 0x80008088, data-base: 0x80F3E4C0  
cisco 2621 (MPC860) processor (revision 0x102) with 36864K/12288K bytes of memory. 
Processor board ID JAD043705GZ (4030259563) 
M860 processor: part number 0, mask 49 Bridging software. X.25 software, Version 3.0.0. 
2 FastEthernet/IEEE 802.3 interface(s) 
1 Serial network interface(s) 
32K bytes of non-volatile configuration memory. 
16384K bytes of processor board System flash (Read/Write)            
--- System Configuration Dialog ---  

Would you like to enter the initial configuration dialog? [yes/no]: n   
Press RETURN to get started! 

Passed 
00:00:20: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up 
00:00:20: %LINK-3-UPDOWN: Interface FastEthernet0/1, changed state to up 
00:00:20: %LINK-3-UPDOWN: Interface Serial0/0, changed state to down 
00:00:21: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up 
00:00:21: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up 
00:00:21: %LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0, changed state to down 
00:01:36: %SYS-5-RESTART: System restarted 

-- Cisco Internetwork Operating System Software IOS (tm) 
C2600 Software (C2600-IS-M), 
Version 12.1(3)XI, EARLY DEPLOYMENT RELEASE SOFTWARE (fc1) 
TAC:Home:SW:IOS:Specials for info Copyright (c) 1986-2000 by cisco Systems, Inc. 
Compiled Sat 29-Jul-00 07:57 by kirthi 

00:01:38: %LINK-5-CHANGED: Interface FastEthernet0/0, changed state to administratively down 
00:01:38: %LINK-5-CHANGED: Interface Serial0/0, changed state to administratively down 
00:01:38: %LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down 
00:01:39: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to down 
00:01:39: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down 
00:01:40: %IP-5-WEBINST_KILL: Terminating DNS process 

Router> 
Router>
```