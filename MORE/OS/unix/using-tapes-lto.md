# using Tapes (LTO)

## Overview: 

## Where is the Tape
You need to figure out if the tape is plugged in and working first.  (And where it's addressed)

Check scsi
```bash
[root@dga1 ~]# cat /proc/scsi/scsi
Attached devices:
[...]
Host: scsi8 Channel: 00 Id: 00 Lun: 00
  Vendor: HP       Model: Ultrium 7-SCSI   Rev: G9Q1
  Type:   Sequential-Access                ANSI  SCSI revision: 06
[root@dga1 ~]#
```
Then list the installed scsi devices
```bash
[root@dga1 ~]# lsscsi -g
[1:0:0:0]    disk    ATA      WDC WD5003ABYZ-0 1S03  /dev/sdi   /dev/sg12
[2:0:0:0]    disk    ATA      WDC WD5003ABYZ-0 1S03  /dev/sdj   /dev/sg13
[7:0:8:0]    enclosu LSI      SAS3x36          0601  -          /dev/sg0
[7:0:9:0]    enclosu LSI      SAS3x40          0601  -          /dev/sg1
[7:0:54:0]   enclosu LSI      SAS3x36          0601  -          /dev/sg2
[7:0:55:0]   enclosu LSI      SAS3x40          0601  -          /dev/sg3
[7:2:0:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sda   /dev/sg4
[7:2:1:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sdb   /dev/sg5
[7:2:2:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sdc   /dev/sg6
[7:2:3:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sdd   /dev/sg7
[7:2:4:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sde   /dev/sg8
[7:2:5:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sdf   /dev/sg9
[7:2:6:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sdg   /dev/sg10
[7:2:7:0]    disk    AVAGO    MR9380-8e        4.68  /dev/sdh   /dev/sg11
[8:0:0:0]    tape    HP       Ultrium 7-SCSI   G9Q1  /dev/st0   /dev/sg14
[root@dga1 ~]#
```

Then confirm the mounts: 

`st` = auto rewind mounts <br>
Every-time you write, you will write from the start of the tape, thus the tape is only good for ONE write.
```bash
[root@dga1 ~]# ll /dev/st*
crw-rw----. 1 root tape 9,  0 Nov 24 17:01 /dev/st0
crw-rw----. 1 root tape 9, 96 Nov 24 17:01 /dev/st0a
crw-rw----. 1 root tape 9, 32 Nov 24 17:01 /dev/st0l
crw-rw----. 1 root tape 9, 64 Nov 24 17:01 /dev/st0m
[...]
[root@dga1 ~]#
```
`nst` = non-rewind mounts: <br>
You can write multiple times to the tape.  
```bash
[root@dga1 ~]# ll /dev/nst*
crw-rw----. 1 root tape 9, 128 Nov 24 17:01 /dev/nst0
crw-rw----. 1 root tape 9, 224 Nov 24 17:01 /dev/nst0a
crw-rw----. 1 root tape 9, 160 Nov 24 17:01 /dev/nst0l
crw-rw----. 1 root tape 9, 192 Nov 24 17:01 /dev/nst0m
[root@dga1 ~]#
```

## Basics

install mt
```bash
[root@dga1 ~]# yum search mt | grep tape
mt-st.x86_64 : Tool for controlling tape drives
rmt.x86_64 : Provides certain programs with access to remote tape devices
[root@dga1 ~]# yum install mt-st
[...]
Installed:
  mt-st.x86_64 0:1.1-14.el7

Complete!
[root@dga1 ~]#
```

confirm the tape is working (with a tape in the drive)
```bash
[root@dga1 ~]# mt -f /dev/st0 status
SCSI 2 tape drive:
File number=-1, block number=-1, partition=0.
Tape block size 0 bytes. Density code 0x5c (no translation).
Soft error count since last status=0
General status bits on (1010000):
 ONLINE IM_REP_EN
[root@dga1 ~]#
```

### Simple copy to tape, list contents of tape, and write from tape to disk
Rewind the tape: 
```bash
[root@dga1 ~]# mt -f /dev/st0 rewind
[root@dga1 ~]#
```

Backup something<br>
In this case the file “epel-release-7-10.noarch.rpm” in root
```bash
[root@dga1 ~]# tar -cvf /dev/st0 epel-release-7-10.noarch.rpm
epel-release-7-10.noarch.rpm
[root@dga1 ~]#
```

list contents of tape: 
```bash
[root@dga1 ~]# tar tvf /dev/st0
-rw-r--r-- root/root     14848 2017-06-24 11:08 epel-release-7-10.noarch.rpm
[root@dga1 ~]#
```

grab something from the tape: <br>
in this case, take a file from tape and put it in the /root/backup directory
```bash
[root@dga1 ~]# tar xvf /dev/st0 -C  backup/
epel-release-7-10.noarch.rpm
[root@dga1 ~]#
```

### Multiple working reads onto one tape
Start off by rewinding the tape
```bash
[root@dga1 backup]# mt -f /dev/st0 rewind
[root@dga1 
```

Write the data: <br>
Copy fist file
```bash
[root@dga1 backup]# tar -cvf /dev/nst0 /root/first.txt
tar: Removing leading '/' from member names
/root/first.txt
[root@dga1 backup]#
```

Move head to end of tape, and copy 2nd file
```bash
[root@dga1 backup]# mt -f /dev/nst0 eof
[root@dga1 backup]# tar -cvf /dev/nst0 /root/second.txt
tar: Removing leading '/' from member names
/root/second.txt
[root@dga1 backup]#
```

Move head to end of tape, and copy 3rd file
```bash
[root@dga1 backup]# mt -f /dev/nst0 eof
[root@dga1 backup]# tar -cvf /dev/nst0 /root/third.txt
tar: Removing leading '/' from member names
/root/third.txt
[root@dga1 backup]#
```

Read the data: 

Rewind the tape to the beginning
```bash
[root@dga1 backup]# mt -f /dev/st0 rewind
[root@dga1 backup]# 
```

checking what was recorded, we see the contents of "file number 0" which was the first file.
```bash
[root@dga1 backup]# mt -f /dev/st0 status
SCSI 2 tape drive:
File number=0, block number=0, partition=0.
Tape block size 0 bytes. Density code 0x5c (no translation).
Soft error count since last status=0
General status bits on (41010000):
 BOT ONLINE IM_REP_EN
[root@dga1 backup]#
[root@dga1 backup]# tar -tvf /dev/nst0
-rw-r--r-- root/root      2966 2018-01-16 12:57 root/first.txt
[root@dga1 backup]#
```

First move the head one forward, then review the status to confirm we're now in slot 1 (Second slot), and confirm that this is the second file.
```bash
[root@dga1 backup]# mt -f /dev/nst0 fsf 1 
[root@dga1 backup]# mt -f /dev/nst0 status
SCSI 2 tape drive:
File number=1, block number=0, partition=0.
Tape block size 0 bytes. Density code 0x5c (no translation).
Soft error count since last status=0
General status bits on (81010000):
 EOF ONLINE IM_REP_EN
[root@dga1 backup]# tar -tvf /dev/nst0
tar: This does not look like a tar archive
tar: Exiting with failure status due to previous errors
[root@dga1 backup]# tar -tvf /dev/nst0
-rw-r--r-- root/root    512679 2018-01-16 12:57 root/second.txt
[root@dga1 backup]#
```

Then the third time: 
```bash
[root@dga1 backup]# mt -f /dev/nst0 fsf 1
[root@dga1 backup]# mt -f /dev/nst0 status
SCSI 2 tape drive:
File number=3, block number=0, partition=0.
Tape block size 0 bytes. Density code 0x5c (no translation).
Soft error count since last status=0
General status bits on (81010000):
 EOF ONLINE IM_REP_EN
[root@dga1 backup]# tar -tvf /dev/nst0
tar: This does not look like a tar archive
tar: Exiting with failure status due to previous errors
[root@dga1 backup]# tar -tvf /dev/nst0
-rw-r--r-- root/root   3076074 2018-01-16 12:58 root/third.txt
[root@dga1 backup]#
```


## References
