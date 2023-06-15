# show tech


Just like cisco's, you can run a `sh tech` command to see a huge collection of debugging information about the state of the switch.

```Shell
# sh tech-support

------------- show version detail -------------

Arista DCS-7280SR-48C6-M-R
Hardware version:    11.04
Deviations:
Serial number:       SFJ37246421
System MAC address:  2899.3a00.0001

Software image version: 4.18.3.1F
Architecture:           i386
Internal build version: 4.18.3.1F-5462878.41831F
Internal build ID:      2052c16e-c85f-4856-85ae-31e76ab41b94

Uptime:                 5 weeks, 4 days, 10 hours and 41 minutes
Total memory:           32463792 kB
Free memory:            27413128 kB
```

The switch also has a scheduler that automatically runs this every hour, and stores 100 of the last sh tech's outputs  (4 days, 4 hours).  

```Shell
# show schedule tech-support
The last CLI command execution was successful
CLI command "show tech-support" is scheduled next at "02:43:56 08/22/2017", interval is 60 minutes
Maximum of 100 log files will be stored
Verbose logging is off
100 log files currently stored in flash:/schedule/tech-support

Start time               Size         Filename
----------------------- ------------- -----------------------------------
Aug 22 2017 01:41       10.0 MB       tech-support_2017-08-22.0141.log.gz
Aug 22 2017 00:41       10.0 MB       tech-support_2017-08-22.0041.log.gz
Aug 21 2017 23:41       10.0 MB       tech-support_2017-08-21.2341.log.gz
Aug 21 2017 22:41       10.0 MB       tech-support_2017-08-21.2241.log.gz
Aug 21 2017 21:41       10.0 MB       tech-support_2017-08-21.2141.log.gz
Aug 21 2017 20:41       10.0 MB       tech-support_2017-08-21.2041.log.gz
Aug 21 2017 19:41       10.0 MB       tech-support_2017-08-21.1941.log.gz
```

You can view these files using the bash command to then run a standard bash command like ls on the /mnt/flash/schedule/tech-support directory.  

```Shell
# bash ls -lF /mnt/flash/schedule/tech-support | more
total 1027236
-rwxrwx--- 1 root eosadmin 10526968 Aug 18 07:43 tech-support_2017-08-18.0740.log.gz*
-rwxrwx--- 1 root eosadmin 10515934 Aug 18 08:43 tech-support_2017-08-18.0840.log.gz*
-rwxrwx--- 1 root eosadmin 10523123 Aug 18 09:43 tech-support_2017-08-18.0940.log.gz*
-rwxrwx--- 1 root eosadmin 10523378 Aug 18 10:43 tech-support_2017-08-18.1040.log.gz*
-rwxrwx--- 1 root eosadmin 10523370 Aug 18 11:43 tech-support_2017-08-18.1140.log.gz*
-rwxrwx--- 1 root eosadmin 10523382 Aug 18 12:43 tech-support_2017-08-18.1240.log.gz*
-rwxrwx--- 1 root eosadmin 10522359 Aug 18 13:43 tech-support_2017-08-18.1340.log.gz*
-rwxrwx--- 1 root eosadmin 10522075 Aug 18 14:43 tech-support_2017-08-18.1440.log.gz*
--More--
```

If you want to view the files directly, you can use all the regular ztools with the bash command

```Shell
# bash zcat /mnt/flash/schedule/tech-support/tech-support_2017-08-18.2040.log.gz | more

------------- show version detail -------------

Arista DCS-7280SR-48C6-M-R
Hardware version:    11.04
Deviations:
Serial number:       SFJ37246421
System MAC address:  2899.3a00.0001

Software image version: 4.18.3.1F
Architecture:           i386
Internal build version: 4.18.3.1F-5462878.41831F
Internal build ID:      2052c16e-c85f-4856-85ae-31e76ab41b94

Uptime:                 5 weeks, 1 day, 5 hours and 7 minutes
Total memory:           32463792 kB
Free memory:            27498420 kB
--More--
```

The easiest way to grab these files is to rsync them off the switch onto your desktop.  

```Shell
# bash rsync --progress -ave ssh /mnt/flash/schedule/tech-support/tech-support_2017-08-17.2340.log.gz me@192.168.30.222:/Users/me/arista/debuging/01/.
Warning: Permanently added '192.168.30.222' (ECDSA) to the list of known hosts.
Password:
building file list ...
1 file to consider
tech-support_2017-08-17.2340.log.gz
     10,523,949 100%    1.79MB/s    0:00:05 (xfr#1, to-chk=0/1)

sent 10,525,374 bytes  received 46 bytes  915,253.91 bytes/sec
total size is 10,523,949  speedup is 1.00
#
```

## References: 
- [How to store and view previous contents of ‘show tech-support’](https://arista.my.site.com/AristaCommunity/s/article/how-to-store-and-view-previous-contents-of-show-tech-support): used in order to automatically save the last 100 outputs of show tech-support (taken every hour)
- [logGrab](https://arista.my.site.com/AristaCommunity/s/article/collecting-logs-for-arista-tac-cases-using-loggrab): script that builds a time/date stamped archive containing a number of log items commonly requested when raising a TAC case
- [Creating a user who logs in directly into bash](https://arista.my.site.com/AristaCommunity/s/question/0D52I00007ERpvkSAD/sftp-scp-rsync-to-eos): How you can remotely rsync to/from the arista
- [enabling sshd on your mac](../../OS/apple/platform-howto/sshing-to-your-box.md): how to enable sshd on a Mac to allow external connections