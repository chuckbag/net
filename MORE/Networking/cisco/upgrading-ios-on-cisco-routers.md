Upgrading IOS on Cisco Routers


## What's the Current Version
Check what your running version is, to see if it's not what you want. Check the amount of memory and your flash disk space

Show Version and Memory

```
! --
router# sh ver
Cisco Internetwork Operating System Software 
IOS (tm) C2600 Software (C2600-IS-M), Version 12.1(13), RELEASE SOFTWARE (fc3) 
Copyright (c) 1986-2002 by cisco Systems, Inc. 
Compiled Wed 30-Jan-02 22:19 by kellythw 
Image text-base: 0x80008088, data-base: 0x80CED5D8 
 
ROM: System Bootstrap, Version 11.3(2)XA4, RELEASE SOFTWARE (fc1) 
  
vpn uptime is 6 weeks, 2 hours, 40 minutes 
System returned to ROM by reload 
System image file is "flash:c2600-is-mz.121-13.bin" 
  
cisco 2620 (MPC860) processor (revision 0x102) with 53248K/12288K bytes of memory. 
Processor board ID JAD04440DOF (1057754228) 
M860 processor: part number 0, mask 49 
Bridging software. 
X.25 software, Version 3.0.0. 
1 Ethernet/IEEE 802.3 interface(s) 
1 FastEthernet/IEEE 802.3 interface(s) 
1 Serial network interface(s) 
32K bytes of non-volatile configuration memory. 
8192K bytes of processor board System flash (Read/Write) 
	    
Configuration register is 0x2102 
  
router# 
router# dir
Directory of flash:/ 
  
    1  -rw-     7701332                c2600-is-mz.121-13.bin 
  
8388608 bytes total (687212 bytes free) 
router#
```

## Get New IOS Image
download the ios image you need from Cisco's site:

http://www.cisco.com/kobayashi/sw-center/sw-ios.shtml

and store it on a tftp server. Remember that the file will need to be world readable to be able to be downloaded from the tftp server.

## Upload New OS on 2600 or 3600 Series Router
Unfortunately, the process for uploading the ios is not (yet) the same for all routers that you will work with. The carrier class routers work like you would imagine. If there is space on the disk, you can copy the new image to it. Then you tell the router to boot the new image in the conf.

On "access" routers, (2600's and 3600's) this is not the way it works. Even if you have a lot of extra disk space, the router is happy only if there is one image on the flash. If this is a 2600 or 3600 series router, the upload process will delete the current image. This is true even if there is enough space for more then one image.

### TFTP new IOS to Router
The first step will be to upload the new ios image to the router. Since we do not have a backup image, it's a good idea to double check the new ios image when it's on the router to make sure that it is ok. Otherwise the router heaves at boot, and you have to reinstall the image through the serial port (u-g-l-y!).

Upload IOS

```
3A01  ! -- upload the ios image to the router.
3A02  c3640-n3-1# copy tftp://172.22.1.203/software/c3640-ik9s-mz.122-23a.bin flash:
3A03  Destination filename [c3640-ik9s-mz.122-23a.bin]? 
3A04  Accessing tftp://172.22.1.203/software/c3640-ik9s-mz.122-23a.bin... 
3A05  Erase flash: before copying? [confirm] 
3A06  Erasing the flash filesystem will remove all files! Continue? [confirm] 
3A07  Erasing device.. eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee 
3A08  eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee ...erased 
3A09  Erase of flash: complete 
3A10  Loading software/c3640-ik9s-mz.122-23a.bin from 172.22.1.203 (via FastEthernet1/0): !!! 
3A11  !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 
3A12  [OK - 12124536 bytes] 
3A13    
3A14  Verifying checksum...  OK (0x266E) 
3A15  12124536 bytes copied in 77.432 secs (156583 bytes/sec) 
3A16   
3A17  ! -- check the flash disk
3A18  c3640-n3-1# dir
3A19  Directory of flash:/ 
3A20    
3A21      1  -rw-    12124536   May 4 2004 14:37:49 -07:00  c3640-ik9s-mz.122-23a.bin 
3A22    
3A23  33030144 bytes total (20905544 bytes free) 
3A24   
3A25  ! -- doublecheck the image
3A26  c3640-n3-1# verify flash:c3640-ik9s-mz.122-23a.bin
3A27  Verified flash:c3640-ik9s-mz.122-23a.bin
3A28  c3640-n3-1# 
3A29  
```

### Specify Image To Boot
Once you are sure that the image is correct and working, you will want to set the configs on the router to boot the new image. Add the new image to the conf, and remove the old one (if there).

Specify New IOS to Boot

```
3B01  ! -- view origional config w/ boot command
3B02  c3640-n3-1# sh run
3B03  Building configuration... 
3B04    
3B05  Current configuration : 6384 bytes 
3B06  ! 
3B07  ! Last configuration change at 13:53:42 PDT Tue Apr 13 2004 
3B08  ! NVRAM config last updated at 13:53:50 PDT Tue Apr 13 2004 
3B09  ! 
3B10  version 12.2 
3B11  no service pad 
3B12  service timestamps debug datetime msec localtime show-timezone 
3B13  service timestamps log datetime msec localtime show-timezone 
3B14  service password-encryption 
3B5  ! 
3B16  hostname c3640-n3-1 
3B17  ! 
3B18  boot system flash:c3640-ik9s-mz.121-13.bin
3B19  [...] 
3B20   
3B21  ! -- re-define boot command
3B22  c3640-n3-1# conf t
3B23  Enter configuration commands, one per line.  End with CNTL/Z. 
3B24  c3640-n3-1(config)# boot system flash:c3640-ik9s-mz.122-23a.bin
3B25  c3640-n3-1(config)# no boot system flash:c3640-ik9s-mz.121-13.bin
3B26  c3640-n3-1(config)# exit
3B27   
3B28  ! -- check work and save
3B29  c3640-n3-1# sh run
3B30  Building configuration... 
3B31    
3B32  Current configuration : 6384 bytes 
3B33  ! 
3B34  ! Last configuration change at 13:53:42 PDT Tue Apr 13 2004 
3B35  ! NVRAM config last updated at 13:53:50 PDT Tue Apr 13 2004 
3B36  ! 
3B37  version 12.2 
3B38  no service pad 
3B39  service timestamps debug datetime msec localtime show-timezone 
3B40  service timestamps log datetime msec localtime show-timezone 
3B41  service password-encryption 
3B42  ! 
3B43  hostname c3640-n3-1 
3B44  ! 
3B45  boot system flash:c3640-ik9s-mz.122-23a.bin
3B46  [...] 
3B47   
3B48  c3640-n3-1# wr mem
3B49  Building configuration... 
3B50  [OK] 
3B51  c3640-n3-1#
```

### Reboot the Router
Finally, reboot the router and check to make sure that the new image took.

Reload and check new loaded OS
```
3C01  ! -- Reboot the box and make sure new os takes
3C02  c3640-n3-1# reload
3C03  Proceed with reload? [confirm] 
3C04   
3C05  [...]  
3C06   
3C07  System Bootstrap, Version 11.1(20)AA2, EARLY DEPLOYMENT RELEASE SOFTWARE (fc1) 
3C08  Copyright (c) 1999 by cisco Systems, Inc. 
3C09  C3600 processor with 131072 Kbytes of main memory 
3C10  Main memory is configured to 64 bit mode with parity disabled 3C11                                                                 
3C12  program load complete, entry point: 0x80008000, size: 0xb90034 
3C13  Self decompressing the image : ############################################################## 
3C14  ################################### [OK] 
3C15   
3C16  [...] 
3C17    
3C18  Cisco Internetwork Operating System Software 
3C19  IOS (tm) 3600 Software (C3640-IK9S-M), Version 12.2(23a), RELEASE SOFTWARE (fc2) 
3C20  Copyright (c) 1986-2004 by cisco Systems, Inc. 
3C21  Compiled Tue 30-Mar-04 12:14 by kellmill 
3C22  Image text-base: 0x60008930, data-base: 0x6129E000 
3C23    
3C24   [...] 
3C25    
3C26  cisco 3640 (R4700) processor (revision 0x00) with 126976K/4096K bytes of memory. 
3C27  Processor board ID 26853049 
3C28  R4700 CPU at 100Mhz, Implementation 33, Rev 1.0 
3C29  Bridging software. 
3C30  X.25 software, Version 3.0.0. 
3C31  SuperLAT software (copyright 1990 by Meridian Technology Corp). 
3C32  2 FastEthernet/IEEE 802.3 interface(s) 
3C33  DRAM configuration is 64 bits wide with parity disabled. 
3C34  125K bytes of non-volatile configuration memory. 
3C35  32768K bytes of processor board System flash (Read/Write) 
3C36   
3C37  Press RETURN to get started! 
3C38   3C39  Password:
```

## Upload New OS on a 7200 Series Router
If this is a 7200 series router, then it saves the image to disk if there is enough room. The nice part about this is that the disk can hold many different images, and you can boot from any of those images. The bad (sort of) part of this, is now you need to understand how to do disk maintenance on the router. (This is not difficult at all.)

### Disk Maintenance
In this section we will look at the disk maintenance part, and then refer to the previous section for the ios uploading, since it is the same for all the routers.

View and remove Unneeded IOS Image
```
3D01  ! -- make sure you know what ios image you are currently running
3D02  c7206-c3-1# sh ver
3D03  Cisco Internetwork Operating System Software 
3D04  IOS (tm) 7200 Software (C7200-IS-M), Version 12.1(7a)E1, EARLY DEPLOYMENT RELEASE SOFTWARE (fc1) 
3D05  TAC Support: http://www.cisco.com/cgi-bin/ibld/view.pl?i=support 
3D06  Copyright (c) 1986-2001 by cisco Systems, Inc. 
3D07  Compiled Tue 08-May-01 10:19 by jamnguye 
3D08  Image text-base: 0x60008950, data-base: 0x61134000 
3D09    
3D10  ROM: System Bootstrap, Version 12.1(20000710:044039) [nlaw-121E_npeb 117], DEVELOPMENT SOFTWARE 
3D11  BOOTFLASH: 7200 Software (C7200-KBOOT-M), Version 12.1(8a)E, EARLY DEPLOYMENT RELEASE SOFTWARE (fc1) 
3D12    
3D13  c7206-c3-1 uptime is 9 weeks, 5 days, 10 hours, 30 minutes 
3D14  System returned to ROM by reload at 00:40:27 UTC Thu Feb 26 2004 
3D15  System image file is "disk0:c7200-is-mz.121-7a.E1" 
3D16    
3D17  cisco 7206VXR (NPE400) processor (revision A) with 245760K/16384K bytes of memory. 
3D18  Processor board ID 26789835 
3D19  R7000 CPU at 350Mhz, Implementation 39, Rev 3.2, 256KB L2, 4096KB L3 Cache 
3D20  6 slot VXR midplane, Version 2.6 
3D21   
3D22  ! -- And what image is defined for boot in the config
3D23  c7206-c3-1# sh bootvar
3D24  BOOT variable = 
3D25  CONFIG_FILE variable does not exist 
3D26  BOOTLDR variable does not exist 
3D27  Configuration register is 0x102 
3D28    
3D29  c7206-c3-1# 
3D30   
3D31  ! -- View the routers disk space 
3D32  c7206-c3-1# dir
3D33  Directory of disk0:/ 
3D34    
3D35      3  -rw-     9978880   Dec 26 2001 17:39:12  c7200-is-mz.121-7a.E1 
3D36   2440  -rw-    12088204   Feb 11 2002 15:12:22  c7200-ik2s-mz.121-5.T5.bin 
3D37    
3D38  47890432 bytes total (25817088 bytes free) 
3D39   
3D40  ! -- delete the unused ios image
3D41  c7206-c3-1# delete disk0:c7200-ik2s-mz.121-5.T5.bin
3D42  Delete filename [c7200-ik2s-mz.121-5.T5.bin]? 
3D43  Delete disk0:c7200-ik2s-mz.121-5.T5.bin? [confirm] 
3D44  c7206-c3-1# dir
3D45  Directory of disk0:/ 
3D46   
3D47      3  -rw-     9978880   Dec 26 2001 17:39:12  c7200-is-mz.121-7a.E1 
3D48    
3D49  47890432 bytes total (37908480 bytes free) 
3D50   
3D51  ! -- "empty the trash"
3D52  c7206-c3-1# squeeze bootflash:
3D53  All deleted files will be removed. Continue? [confirm] 
3D54  Squeeze operation may take a while. Continue? [confirm] 
3D55                   
3D56  Squeeze of bootflash complete 
3D57  c7206-c3-1#
```

Next we need to upload the new ios on to the router. These are the same procedures as the steps above.

First upload the new ios, and then reboot the router.

## Appendix 1: References
- Cisco 7200 Series Routers: PCMCIA Filesystem Compatibility Matrix and Filesystem Information
- How to Choose a Cisco IOSÂ® Software Release
- White Paper: Cisco IOS Reference Guide
- Configuration Management: Best Practices White Paper