# Upgrading the ios on a Cat Switch

- [Upgrading the ios on a Cat Switch](#upgrading-the-ios-on-a-cat-switch)
  - [Check the current version](#check-the-current-version)
  - [Check the flash for room](#check-the-flash-for-room)
  - [Download the new os to the switch](#download-the-new-os-to-the-switch)
  - [Check the flash for new file](#check-the-flash-for-new-file)
  - [Verify the new os is ok](#verify-the-new-os-is-ok)
  - [Check current configs](#check-current-configs)
  - [Set the correct ios to boot](#set-the-correct-ios-to-boot)
  - [Reboot to new os](#reboot-to-new-os)
  - [Checking the running version](#checking-the-running-version)
  - [Delete old OS](#delete-old-os)
  - [Appendix A: Errors](#appendix-a-errors)
    - [Bad Boot OS](#bad-boot-os)
  - [Appendix B: References](#appendix-b-references)


## Check the current version
To find out what version os, you are currently running, type `sh ver` . For this example the switch is running on version 6.1(1) code. 

check current catos version

```
01  c4003-devlab-1> (enable) sh ver
02  WS-C4003 Software, Version NmpSW: 6.1(1)
03  Copyright (c) 1995-2000 by Cisco Systems, Inc. 
04  NMP S/W compiled on Sep 30 2000, 12:25:52 
05  GSP S/W compiled on Sep 28 2000, 18:28:28 
06  
07  System Bootstrap Version: 5.4(1) 
08  
09  Hardware Version: 2.1  Model: WS-C4003  Serial #: JAE050703SJ 
10  
11  Mod Port Model      Serial #              Versions 
12  --- ---- ---------- -------------------- --------------------------------- 
13  1   0    WS-X4012   JAE050703SJ          Hw : 2.1 
14                                           Gsp: 6.1(1.0) 
15                                           Nmp: 6.1(1) 
16  2   34   WS-X4232-GB-RJ JAE0441043G      Hw : 2.3 
17  3   48   WS-X4148-RJ JAE0435013J         Hw : 2.3 
18  
19         DRAM                    FLASH                   NVRAM 
20  Module Total   Used    Free    Total   Used    Free    Total Used  Free 
21  ------ ------- ------- ------- ------- ------- ------- ----- ----- ----- 
22  1      65536K  27270K  38266K  12288K   9045K   3243K  480K  171K  309K 
23  
24  Uptime is 0 day, 0 hour, 7 minutes
```

## Check the flash for room
space in flash

```
01  c4003-devlab-1> (enable)  sh flash
02 -#- ED --type-- --crc--- -seek-- nlen -length- -----date/time------ name 
03   1 .. ffffffff 9931da99  3fc33c   20  3916476 Feb 11 2002 14:49:18 cat4000-k9.6-1-2.bin 
04  05 7617732 bytes available (3916604 bytes used)
```

## Download the new os to the switch

We are downloading the new os from a tftp server. The files location is at the URL: `tftp://172.22.1.202/software/cat4000-k9.7-1-1.bin`

download new catos

```
01  c4003-devlab-1> (enable) copy tftp flash  
02  IP address or name of remote host [10.10.82.57]?  64.210.178.200  
03  Name of file to copy from [cat4000-k9.6-1-2.bin]? software/cat4000-k9.7-1-1.bin  
04  Flash device [bootflash]?  
05  Name of file to copy to [cat4000-k9.7-1-1.bin]?  
06  
07  7617604 bytes available on device bootflash, proceed (y/n) [n]? y  
08  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC 
09  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC 
10  File has been copied successfully.  
11  c4003-devlab-1> (enable) 
```

## Check the flash for new file
Make sure that the file downloaded ok

check for download

```
01  c4003-devlab-1> (enable) sh flash
02  -#- ED --type-- --crc--- -seek-- nlen -length- -----date/time------ name 
03    1 .. ffffffff 3fb14273  3e4084   17  3817476 Feb 24 2001 18:18:37 cat4000.6-1-1.bin 
04    2 .. ffffffff 7a0294b7  8151dc   20  4395224 Feb 08 2002 16:34:02 cat4000-k9.7-1-1.bin
05   
06  3321380 bytes available (8212956 bytes used) 
07  c4003-devlab-1> (enable) 
```


## Verify the new os is ok
This checks the crc, and makes sure that the file was downloaded ok.

check quality of download

```
01  c4003-devlab-1> (enable)  verify bootflash:cat4000-k9.7-1-1.bin
02  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC 
03  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC 
04  File bootflash:cat4000-k9.7-1-1.bin verified OK
05  c4003-1> (enable)
```

## Check current configs
You will need to change the configs so that the cat boots from the new os, here, just make sure that the cat was told to boot from a specific ios version, instead of the first file in flash, of from the network.

check config

```
01  c4003-devlab-1> (enable) wr term
02  This command shows non-default configurations only. 
03  Use 'write terminal all' to show both default and non-default configurations. 
04  ....... 
05   
06  .................... 
07   
08  .. 
09   
10  begin 
11  ! 
12  [... some lines removed ...] 
13  ! 
14  ! -- set boot command
15  set boot config-register 0x2
16  set boot system flash bootflash:cat4000.6-1-1.bin
17  !
```


## Set the correct ios to boot
Make the switch boot from the new ios version. First tell the switch the new ios to boot from.

set new catos to load

```
01  c4003-devlab-1> (enable) set boot system flash bootflash:cat4000-k9.7-1-1.bin
02  BOOT variable = bootflash:cat4000.6-1-1.bin,1;bootflash:cat4000-k9.7-1-1.bin,1; 
03  c4003-devlab-1> (enable)  
04   
05  ! -- Then clear the old command telling it to boot from the old os
06  ! -- clear old boot command
07  c4003-devlab-1> (enable) clear boot system flash bootflash:cat4000.6-1-1.bin  
08  BOOT variable = bootflash:cat4000-k9.7-1-1.bin,1; 
09  c4003-devlab-1> (enable)  
10   
11  ! -- and if your paranoid, you can double check the configs to make sure that that they took.
12   
13  c4003-devlab-1> (enable) wr term
14  This command shows non-default configurations only. 
15  Use 'write terminal all' to show both default and non-default configurations. 
16  ....... 
17   
18  .................... 
19   
20  .. 
21   
22  begin 
23  ! 
24  [... some lines removed ...] 
25  ! 
26  ! -- set boot command
27  set boot config-register 0x2
28  set boot system flash bootflash:cat4000-k9.7-1-1.bin
29  !
```

## Reboot to new os
The switch must reboot to run the new catios. Yes, there will be an outage while this happens.

reboot

```
01  c4003-devlab-1> (enable)  reset
02  This command will reset the system. 
03  Do you want to continue (y/n) [n]? y
04  2002 Feb 11 11:09:09 %SYS-5-SYS_RESET:System reset from Console// 
05  c4003-dev0:00.524447: Please set IPAddr variable 
06  0:00.525026: Please set Netmask variable 
07  0:00.525387: Please set Broadcast variable 
08  0:00.526025: Please set TftpServer variable to do tftp downloads 
09  0:00.526571: Network is not configured 
10  WS-X4012 bootrom version 5.4(1), built on 2000.04.04 10:48:54 
11  H/W Revisions:    Meteor: 4    Comet: 8    Board: 2 
12  Supervisor MAC addresses: 00:05:32:69:ce:00 through 00:05:32:69:d1:ff (1024 addresses) 
13  Installed memory: 64 MB 
14  Testing LEDs.... done! 
15  The system will autoboot in 5 seconds. 
16  Type control-C to prevent autobooting. 
17  rommon 1 > 
18  The system will now begin autobooting. 
19  Autobooting image: "bootflash:cat4000-k9.7-1-1.bin" 
20  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC 
21  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC######################## 
22  ######## 
23  Starting Off-line Diagnostics 
24  Mapping in TempFs 
25  Board type is WS-X4012 
26  DiagBootMode value is "post" 
27  Loading diagnostics... 
28   
29  Power-on-self-test for Module 1:  WS-X4012 
30  Status: (. = Pass, F = Fail) 
31  processor: .           cpu sdram: .           eprom: . 
32  nvram: .               flash: .               enet console port: . 
33  switch port 0: .       switch port 1: .       switch port 2: . 
34  switch port 3: .       switch port 4: .       switch port 5: . 
35  switch port 6: .       switch port 7: .       switch port 8: . 
36  switch port 9: .       switch port 10: .      switch port 11: . 
37  switch registers: .    switch sram: . 
38  Module 1 Passed 
39   
40  Exiting Off-line Diagnostics 
41  Update NVRAM successful 
42  Update NVRAM successful 
43  Update NVRAM successful 
44   
45   
46  Cisco Systems, Inc. Console
```

## Checking the running version
Just incase something didn't look right at boot, you can check the running os again to make sure that the new ios took:

check running ios version

```
01  c4003-devlab-1> (enable) sh ver
02  WS-C4003 Software, Version NmpSW: 7.1(1)
03  Copyright (c) 1995-2001 by Cisco Systems, Inc. 
04  NMP S/W compiled on Nov 30 2001, 12:48:47 
05  GSP S/W compiled on Nov 30 2001, 11:41:54 
06   
07  System Bootstrap Version: 5.4(1) 
08   
09  Hardware Version: 2.1  Model: WS-C4003  Serial #: JAE050703SJ 
10   
11  Mod Port Model              Serial #              Versions 
12  --- ---- ------------------ -------------------- --------------------------------- 
13  1   0    WS-X4012           JAE050703SJ          Hw : 2.1 
14                                                   Gsp: 7.1(1.0) 
15  						                         Nmp: 7.1(1) 
16  2   34   WS-X4232-GB-RJ     JAE0441043G          Hw : 2.3 
17  3   48   WS-X4148-RJ        JAE0435013J          Hw : 2.3 
18   
19         DRAM                    FLASH                   NVRAM 
20  Module Total   Used    Free    Total   Used    Free    Total Used  Free 
21  ------ ------- ------- ------- ------- ------- ------- ----- ----- ----- 
22  1      65536K  34458K  31078K  12288K   9045K   3243K  480K  266K  214K 
23   
24  Uptime is 0 day, 0 hour, 9 minutes 
25  c4003-devlab-1> (enable)
```

## Delete old OS
You might want to wait on this step until you are sure that the new os is what you want. (maybe a couple of days to see if any bugs pop up.)

Look at the directory to get the correct filename.

delete old catos
```
01  c4003-devlab-1> (enable)  dir
02  -#- -length- -----date/time------ name 
03    1  3916476 Feb 11 2002 14:49:18 cat4000-k9.6-1-2.bin 
04    2  4395224 Feb 11 2002 18:07:09 cat4000-k9.7-1-1.bin 
05   
06  3222380 bytes available (8311956 bytes used) 
07  c4003-devlab-1> (enable) 08   

09  Delete the old os: 
10   
11  c4003-devlab-1> (enable)  delete cat4000-k9.6-1-2.bin
12  c4003-devlab-1> (enable)  
13   
14  Then squeeze the file to "empty the trash". 
15   
16  c4003-devlab-1> (enable)  squeeze bootflash:
17   
18  All deleted files will be removed, proceed (y/n) [n]? y
19   
20  Squeeze operation may take a while, proceed (y/n) [n]? y
21  Erasing squeeze log 
22  c4003-devlab-1> (enable) 
```

## Appendix A: Errors
### Bad Boot OS
This console output is what will happen if your boot os is not good, and you don't have a backup.

error
```
01  The system will now begin autobooting. 
02  Autobooting image: "bootflash:cat4000-k9.7-1-1.bin" 
03  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC 
04  CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCloadprog: error - on file
05  open 
06  Booting "bootflash:cat4000-k9.7-1-1.bin" failed. (loadprog() returned -1.) 
07  Resetting system... 
08  0:00.523109: Please set IPAddr variable 
09  0:00.523689: Please set Netmask variable 
10  0:00.524059: Please set Broadcast variable 
11  0:00.524696: Please set TftpServer variable to do tftp downloads 
12  0:00.525241: Network is not configured 
13  WS-X4012 bootrom version 5.4(1), built on 2000.04.04 10:48:54 
14  H/W Revisions:    Meteor: 4    Comet: 8    Board: 2 
15  Supervisor MAC addresses: 00:05:32:69:ce:00 through 00:05:32:69:d1:ff (1024 addr 
16  esses) 
17  Installed memory: 64 MB 
18  Testing LEDs.... done! 
19  The system will autoboot in 5 seconds. 
20  Type control-C to prevent autobooting. 
21  rommon 1 >
```

## Appendix B: References
You will want to download the new code from cisco, and then keep it on a tftp server where the switches will have access to. The following links should be helpful in finding the correct ios, and answering questions about the different ios versions.

- Cisco LAN Switching Software. This page lists all the different switch os's and links to how to download them.
- Catalyst 4000 Family Switches. Hardware and software documentation for the Catalyst 4000 family, 4003, 4006, 4912G, 2926G, 2948G, and Catalyst 2980G switches.
- Catalyst 5000 Family Switches. Hardware and software documentation for the Catalyst 5500 series (5500, 5509, & 5505) and Catalyst 5000 series (5000 & 5002) switches.
- Cisco IOS Upgrade Planner. You can view all major releases, all platforms, and all software features from a single interface.
- Recovering the Catalyst 4000, Catalyst 2948G, Catalyst 2980G, and Catalyst 4912G. If you encounter a problem with the software you currently have in Flash, and are unable to fully boot-up the switch, or if the switch ends up in ROM monitor for any reason and you need to get the switch back up, you can now perform a netboot using the procedure described herein.
- How to Configure SSH on Catalyst Switches Running CatOS.

