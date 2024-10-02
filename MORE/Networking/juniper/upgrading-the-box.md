# Upgrading the Box

## Overview: 
This is how you upgrade a non-HA JunOS system.

## Procedures: 
Download your favorite OS version from Juniper.  Then scp it to the juniper box from your local system. Note that the highlighted path is the file 
```bash
> scp junos-srxsme-12.1X44-D40.2-domestic.tgz 10.33.195.18:/cf/var/tmp/junos-srxsme-12.1X44-D40.2-domestic.tgz
user@10.33.195.18's password:
junos-srxsme-12.1X44-D40.2-domestic.tgz     7%   10MB   1.3MB/s   01:39 ETA
```

Once it has been copied to the Juniper box, enter the following at the cli prompt.  This should take the ziped file from tmp, install it, and reboot the firewall to run the new OS.  
```bash
user@fw> request system software add /var/tmp/junos-srxsme-12.1X44-D40.2-domestic.tgz no-copy no-validate reboot
```

From the console, you can see the OS that is being loaded: 
```bash
Booting [/kernel]...
Kernel entry at 0x801000e0 ...
init regular console
Primary ICache: Sets 64 Size 128 Asso 4
Primary DCache: Sets 1 Size 128 Asso 64
Secondary DCache: Sets 128 Size 128 Asso 8
GDB: debug ports: uart
GDB: current port: uart
KDB: debugger backends: ddb gdb
KDB: current backend: ddb
kld_map_v: 0x8ff80000, kld_map_p: 0x0
Copyright (c) 1996-2014, Juniper Networks, Inc.
All rights reserved.
Copyright (c) 1992-2006 The FreeBSD Project.
Copyright (c) 1979, 1980, 1983, 1986, 1988, 1989, 1991, 1992, 1993, 1994
        The Regents of the University of California. All rights reserved.
JUNOS 12.1X44-D40.2 #0: 2014-08-28 12:20:14 UTC
```

And once you log back into the firewall (after reboot) you will be shown the OS version directly, but can also get it with the show systems software command. 
```bash
--- JUNOS 12.1X44-D40.2 built 2014-08-28 12:20:14 UTC
user@fw> 
user@fw> show system software
Information for junos:

Comment:
JUNOS Software Release [12.1X44-D40.2]
```

If there was a problem, you can confirm the file was properly downloaded to the juniper box via going out to shell, and then doing a directory listing: 
```bash
user@fw> start shell
% ls -lF /cf/var/tmp/
total 292264
-rw-r--r--  1 root      wheel       3796 Aug 24 22:42 cleanup-pkgs.log
-rw-r--r--  1 root      wheel          0 Aug 24 21:31 eedebug_bin_file
-rw-r--r--  1 root      wheel         34 Aug 24 21:31 gksdchk.log
drwxr-xr-x  2 root      wheel        512 Jun 13 03:20 gres-tp/
-rw-r--r--  1 root      wheel          4 Aug 24 22:43 idp_license_info
drwxrwxrwx  2 root      wheel        512 Jun 13 03:18 install/
-rwx------  1 user      wheel  149551395 Aug 25 05:13 junos-srxsme-12.1X44-D40.2-domestic.tgz*
-rw-r--r--  1 root      wheel        155 Aug 24 22:43 krt_gencfg_filter.txt
drwxrwxrwx  2 root      wheel        512 Jun 13 03:18 pics/
drwxr-xr-x  2 root      wheel        512 Jun 13 03:20 rtsdb/
-rw-r--r--  1 root      wheel          0 Jun 13 03:18 spu_kmd_init
drwxrwxrwt  2 root      wheel        512 Jun 13 03:18 vi.recover/
% cli
user@fw>
```

## References: 
- [Example: Installing Junos OS Upgrades on SRX Series Devices](http://www.juniper.net/documentation/en_US/junos12.1/topics/example/security-junos-os-upgrade-srx-series-device-installing.html): Junos 12.1, Oct.2012
- [file copy](http://www.juniper.net/documentation/en_US/junos13.3/topics/reference/command-summary/file-copy.html): Junos 13.3, Jan 2014