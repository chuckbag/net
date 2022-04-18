# Centos-basicServer

## Network Install: 
Download Network Install iso from CentOS mirror:
- Boot from iso
- Specify http for network install
  - server: mirrors.rit.edu
  - path: centos/5.5/os/i386
- Blow away old partitions and let centos define partition for you.
- Define your network, and hostname
- wait.

Get all the basic things up and working: 


## Minimal CD Install: 
boot from minimal iso (7 in these notes)

once complete, do the following 

### Confirm network is working
check what your ip address is with ip (ifconfig is deprecated)
```bash
ip addr
```

Confirm you have a path out so you can update the imagevirtualbo
```bash
ping 8.8.8.8
```

### Static IPs
confirm available interfaces and their names
```bash
ip a
```

edit the file
```bash
vim /etc/sysconfig/network-scripts/ifcfg-eth0
```

where "eth0" is the interface you confirmed you want to modify from the previous step, and then make the following edits to the file
```
DEVICE=eth0
BOOTPROTO=none
ONBOOT=yes
PREFIX=24
IPADDR=192.168.2.203
GATEWAY=192.168.2.1
DNS1=8.8.8.8
DNS2=8.8.4.4
DEFROUTE=yes
```

and then reset the interface with the command 
```bash
systemctl restart network
```

and check the interface again with the command
```bash
ip addr
```

### DHCP
you can reset your IP via dhcp 

release IP
```bash
dhclient -v -r eth0
```

renew IP
```bash
dhclient -v eth0
```

### update the OS
Update the host with all the latest patches 
```bash
yum -y update
```

Throw on a couple of apps to make the box easier to work with
```bash
yum install net-tools ntp vim net-snmp-utils curl screen bind-utils rsync
```

### Modify hostname
use hostnamectl command to view system info including the hostname
```bash
[root@localhost ~]# hostnamectl
   Static hostname: localhost.localdomain
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 28e81936b19d4291b2cf81ecd277b0a3
           Boot ID: d1a573ee68544596b1b85f2bc94bae98
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-693.el7.x86_64
      Architecture: x86-64
[root@localhost ~]#
```

modify the hostname 
```bash
[root@localhost ~]# hostnamectl set-hostname mon01
```

and then confirm it took
```bash
[root@localhost ~]# hostnamectl status
   Static hostname: mon01
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 28e81936b19d4291b2cf81ecd277b0a3
           Boot ID: d1a573ee68544596b1b85f2bc94bae98
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-693.el7.x86_64
      Architecture: x86-64
[root@localhost ~]#
```

then relog into the console to get the updated prompt
```bash
[root@mon01 ~]#
```

## Other Notes: 

### Installing Apps with Yum:
see yum page for more details
- `yum -y check-update`
- `yum -y list installed` lists all apps installed on host
- `yum -y list` downloads header info of all apps available for install and stores it locally
- `yum -y update` upgrades all out of date apps.
- `yum search apache` search what is available for install with the name "apache"
- `yum install net-snmp-utils.x86_64` installs the app net-snmp-utils.x86_64

### Installing Apps with RPMs:
```bash
rpm -i -UVh <filename.rpm> Install a rpm  
```

Starting and stopping services v5:
```bash
service iptables start|stop|status|restart will do whatever to the iptables service.
```

## Configure NTP: 
```bash
yum install ntp
ntpdate 0.pool.ntp.org
chkconfig ntpd --add
```

then confirm that it's working: 
```bash
/usr/sbin/ntpq
ntpq> peers
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+216.45.57.38    216.218.192.202  2 u   13   64  377   99.620    4.221   1.846
*ccadmin.cycores 130.207.244.240  2 u   11   64  377   36.277    1.260   1.190
+dns01.iar.clt.n 64.250.177.145   2 u   11   64  377   43.431    0.053   0.634
 LOCAL(0)        .LOCL.          10 l    7   64  377    0.000    0.000   0.001
ntpq>
ntpq> rv
assID=0 status=c624 sync_alarm, sync_ntp, 2 events, event_peer/strat_chg,
version="ntpd 4.2.2p1@1.1570-o Fri Nov 18 13:21:21 UTC 2011 (1)",
processor="x86_64", system="Linux/2.6.18-348.el5", leap=11, stratum=16,
precision=-20, rootdelay=0.000, rootdispersion=7.125, peer=48701,
refid=ØB, reftime=00000000.00000000  Thu, Feb  7 2036  1:28:16.000,
poll=6, clock=d56cdee7.54208f96  Wed, Jun 19 2013 21:52:39.328, state=3,
offset=3.385, frequency=0.000, jitter=2.053, noise=1.440,
stability=0.000, tai=0
ntpq>
```

the peers statement will show you who you are polling and what their status is, and the rv statement will tell you what you are.  Right off the bat, it will say stratum 16 (not working), but after a few min, it will settle into something good.  


