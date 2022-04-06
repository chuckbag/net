# tftp



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
