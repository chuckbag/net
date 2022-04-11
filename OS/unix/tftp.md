# tftp

- [tftp](#tftp)
	- [tftp on Centos](#tftp-on-centos)
	- [Installing a tftp server (tftpd) on a gentoo host](#installing-a-tftp-server-tftpd-on-a-gentoo-host)
	- [References:](#references)

## tftp on Centos

Add Service: 
```bash
yum install tftp-server
```

Setup rights for the tftpd service: 
```bash
/usr/sbin/adduser tftpd
mkdir /home/tftpd/tftpboot
chown tftpd:tftpd /home/tftpd/tftpboot/
rmdir /tftpboot
```

Add acl for IPTables: (this will ensure that tftp over udp:69 is open on the box)
```bash
/sbin/iptables -I INPUT -p udp --dport 69 -j ACCEPT
/sbin/service iptables save
/sbin/service iptables restart
```

Make sure that tftpd starts on boot: 
```bash
/sbin/chkconfig xinetd on
```

Fix the startup (xinetd) script: 
```bash
vim /etc/xinetd.d/tftp
```

Enable the service by changing "`disable`", set the user to tftp with the "`-u`" option, make all files R/W by user/group with `-U 117`, and set the path for the tftp directory with the "`-s`" flag.  Note that with this config, you can not upload new files, only write over current files.  If you want to add new files use the "`-c`" arg. 
```
     server_args             = -u tftpd -U 117 -s /home/tftpd/tftpboot
     disable                 = no
```

Then startup the tftpd service: 
```bash
/sbin/service xinetd start
```



## Installing a tftp server (tftpd) on a gentoo host
These are the steps to take to install a tftp server on to a host with a [gentoo](https://www.gentoo.org/) distro.

Install the BSD hpa-tftpd server:
```
emerge net-ftp/tftp-hpa
```
Alter the config file with the correct root dir, and put it in a chroot jail.
```
vim /etc/conf.d/in.tftpd
	INTFTPD_OPTS="-R 4096:32767 -s /tftpboot/"
```

Create the tftp directory:
```
mkdir /tftpboot
```
Start the tftp deamon
```
/etc/init.d/in.tftpd start
```
Set the deamon to start automaticly each time the host boots up.
```
rc-update add in.tftpd default
```

## References: 
- [CentOS 6.3: Installing a TFTPD server for uploading configuration files](http://n40lab.wordpress.com/2013/01/29/centos-6-3-installing-a-tftpd-server-for-uploading-configuration-files/)
- [man tftpd](http://linux.die.net/man/8/tftpd) 
- [Installing a tftp server (tftpd) on a gentoo host](http://net.cmed.us/Home/unixlinux/tftpd)
- [tftpd64](http://tftpd64.software.informer.com/): Client/server for windows box.  

