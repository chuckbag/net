# BIND9-Chrooted on Centos7

## Install the binaries

Install the chrooted version of bind
``` bash
yum -y install bind-chroot
```

initialize the chrooted version of named, and make sure that the non-chrooted versions are not active. 
```txt
/usr/libexec/setup-named-chroot.sh /var/named/chroot on
systemctl stop named
systemctl disable named
systemctl start named-chroot
systemctl enable named-chroot
ln -s '/usr/lib/systemd/system/named-chroot.service' '/etc/systemd/system/multi-user.target.wants/named-chroot.service'
```

once you make the above changes, the bind directories should all be setup properly.  Confirm and make sure they all have the following files/directories: 
```bash
[root@ns02 ~]# ll /var/named/chroot/etc
total 688
-rw-r--r--. 10 root root     118 Jan 30 08:49 localtime
drwxr-x---.  2 root named      6 Jan 22 13:30 named
-rw-r-----.  1 root named   1705 Mar 22  2016 named.conf
-rw-r--r--.  1 root named   3923 Jan 22 13:30 named.iscdlv.key
-rw-r-----.  1 root named    931 Jun 21  2007 named.rfc1912.zones
-rw-r--r--.  1 root named   1587 May 22  2017 named.root.key
drwxr-x---.  3 root named     25 Feb 25 01:50 pki
-rw-r--r--.  1 root root    6545 Jun  7  2013 protocols
-rw-r-----.  1 root named     77 Feb 25 01:56 rndc.key
-rw-r--r--.  1 root root  670293 Jun  7  2013 services
[root@ns02 ~]#
```
```bash
[root@ns02 ~]# ll /var/named/chroot/var/named
total 16
drwxr-x---. 7 root  named   61 Feb 25 01:50 chroot
drwxrwx---. 2 named named   23 Feb 25 01:56 data
drwxrwx---. 2 named named   60 Feb 25 01:56 dynamic
-rw-r-----. 1 root  named 2281 May 22  2017 named.ca
-rw-r-----. 1 root  named  152 Dec 15  2009 named.empty
-rw-r-----. 1 root  named  152 Jun 21  2007 named.localhost
-rw-r-----. 1 root  named  168 Dec 15  2009 named.loopback
drwxrwx---. 2 named named    6 Jan 22 13:30 slaves
[root@ns02 ~]#
```

Create the directories for the zone files which will be created later
```bash
touch /var/named/chroot/var/named/data/cache_dump.db
touch /var/named/chroot/var/named/data/named_stats.txt
touch /var/named/chroot/var/named/data/named_mem_stats.txt
touch /var/named/chroot/var/named/data/named.run
mkdir /var/named/chroot/var/named/dynamic
mkdir /var/named/chroot/etc/named/zones
touch /var/named/chroot/var/named/dynamic/managed-keys.bind
chmod -R 777 /var/named/chroot/var/named/data
chmod -R 777 /var/named/chroot/var/named/dynamic
```


## named.conf
The file `/var/named/chroot/etc/named.conf` file is where you store the configs for who can do zone transfers, and where the zone files are stored.  

copy the original file to the chrooted location
```bash
cp -p /etc/named.conf /var/named/chroot/etc/named.conf
```
Edit the named.conf file
```bash
vim /var/named/chroot/etc/named.conf
```
Make sure you enter in the IP addresses that bind will listen on.  (just add the servers IP)
```txt
        listen-on port 53 {     127.0.0.1;
                                198.18.3.10;
        };

Define what networks can talk to the server
        allow-query             { localhost;
                                  198.18.0.0/15;
        };
        allow-query-cache       { localhost;
                                  198.18.0.0/15;
        };
        forwarders              { 8.8.8.8;
                                  8.8.4.4;
        };
```

Add the following includes (at the bottom of the config file) to tell the bind server where to look for resolving names: 
```
include "/etc/named/named.conf.local";
```

## named.conf.local
This used to be just a part of the named.conf file, but we're breaking it out to a separate file to help keep everything cleaner.  Create the new file 
```bash
vim /var/named/chroot/etc/named/named.conf.local
```

and add the following
```txt
# Forward Zone Files:

zone "cmed.us" {
  type master;
  file "/etc/named/zones/db.cmed.us"; # primary zone file
};


# Reverse Zone Files:
zone "0.18.198.in-addr.arpa" {
  type master;
  file "/etc/named/zones/db.198.18.0"; # outside network
};

zone "1.18.198.in-addr.arpa" {
  type master;
  file "/etc/named/zones/db.198.18.1"; # dmz network
};
```

## Forward Zone File
You can have a single zone file for all of your hosts that are in the same namespace.
Create the file noted above in the .local file: 
```bash
vim /var/named/chroot/etc/named/zones/db.cmed.us
```

and add the following header to the file.  Note that `ns1.cmed.us.` is the fqdn of this server, and `admin.cmed.us.` is the email admin email address associated to the domain.  Note that a semi-colon `;` is the beginning of a comment.  Every time you modify this file, you will need to update the "Serial" number.  To keep track of this, we use the date format for the serial number of the change.  
```txt
$ORIGIN .
$TTL 38400      ; 10 hours 40 minutes
cmed.us         IN      SOA     ns01.cmed.us. abuse.cmed.us. (
           2018010401   ; Serial YYYYMMDD##
                10800   ; Refresh after 3 hours
                3600    ; Retry after 1 hour
                604800  ; Expire after 1 week
                86400 ) ; Minimum TTL of 1 day

; the following is within the X.cmed.us zone.
$ORIGIN cmed.us.

; name servers - NS records
                IN      NS      ns01.cmed.us.
```

and then enter in each entry 
```txt
vzn             IN      A       198.18.0.1
dvr             IN      A       198.18.0.100
```

all entries are in the following format: 
- {`hostname`} `IN A` {`IP`} for A Records
- {`alias`} `IN CNAME` {`hostname`} for CNAME Records

## Reverse Zone Files
These files are separated by networks, not by domains.  You will need to create one for each subnet.  
```bash
vim /var/named/chroot/etc/named/zones/db.198.18.0
```

Enter in the following for the header of the file.  If the network is "198.18.0.0/24", then the "in-addr.arpa" section is "0.18.198" (you reverse the network). 
```txt
$ORIGIN .
$TTL 86400              ; 1 day

0.18.198.in-addr.arpa       IN      SOA     ns1.cmed.us. admin.cmed.us. (
         2018010401     ; Serial YYYYMMDD##
              43200     ; Refresh    (12 hours)
                900     ; Retry      (15 min)
            1814400     ; Expire     (3 weeks)
              10800     ; Min        (3 hours)
)

; name servers - NS records
        IN      NS      ns1.cmed.us

$ORIGIN 0.18.198.in-addr.arpa.
```

and then enter in each entry
```txt
1       IN      PTR     vzn.cmed.us.
100     IN      PTR     dvr.cmed.us.
254     IN      PTR     out-fw01.cmed.us.
```

## Start/Restart Service
### Check files
Start by getting within the chroot directory: 
```bash
[root@ns01 ~]# cd /var/named/chroot
```


First confirm that all the named.conf* files are correct.
```bash
[root@ns01 chroot]# named-checkconf
[root@ns01 chroot]#
```


Then confirm that the forward zone files are correct, where "cmed.us" is the domain, and "/etc/named/zones/db.cmed.us" is the forward zone file.  
```bash
[root@ns01 chroot]# named-checkzone cmed.us etc/named/zones/db.cmed.us
/etc/named/zones/db.cmed.us:13: record with inherited owner (cmed.us) immediately after $ORIGIN (cmed.us)
zone cmed.us/IN: loaded serial 2018010401
OK
[root@ns01 chroot]#
```

and confirm that the reverse zone files are correct.  where "0.18.198.in-addr.arpa" is the reverse for the zone, and "/etc/named/zones/db.198.18.0" is the file to check
```bash
[root@ns01 chroot]# named-checkzone 0.18.198.in-addr.arpa etc/named/zones/db.198.18.0
zone 0.18.198.in-addr.arpa/IN: loaded serial 2018010401
OK
[root@ns01 chroot]#
```

### Start Bind
```bash
[root@ns01 ~]# systemctl start named-chroot
[root@ns01 ~]# systemctl enable named-chroot
Created symlink from /etc/systemd/system/multi-user.target.wants/named.service to /usr/lib/systemd/system/named.service.
[root@ns01 ~]#
[root@ns01 ~]# systemctl status named-chroot
● named.service - Berkeley Internet Name Domain (DNS)
   Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-01-05 16:55:45 GMT; 26s ago
 Main PID: 23453 (named)
   CGroup: /system.slice/named.service
           └─23453 /usr/sbin/named -u named -c /etc/named.conf

Jan 05 16:55:45 ns01 named[23453]: zone 1.0.0.127.in-addr.arpa/IN: loaded serial 0
Jan 05 16:55:45 ns01 named[23453]: zone localhost/IN: loaded serial 0
Jan 05 16:55:45 ns01 named[23453]: zone 3.18.198.in-addr.arpa/IN: loaded serial 2018010401
Jan 05 16:55:45 ns01 named[23453]: zone 0.18.198.in-addr.arpa/IN: loaded serial 2018010401
Jan 05 16:55:45 ns01 named[23453]: zone 1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.i...ial 0
Jan 05 16:55:45 ns01 named[23453]: zone localhost.localdomain/IN: loaded serial 0
Jan 05 16:55:45 ns01 named[23453]: zone 1.18.198.in-addr.arpa/IN: loaded serial 2018010401
Jan 05 16:55:45 ns01 named[23453]: zone 2.18.198.in-addr.arpa/IN: loaded serial 2018010401
Jan 05 16:55:45 ns01 named[23453]: all zones loaded
Jan 05 16:55:45 ns01 named[23453]: running
Hint: Some lines were ellipsized, use -l to show in full.
[root@ns01 ~]#
```

## Firewall Configurations
You need to enable queries into the server, so you will need to modify the local firewall rules by entering the following
```bash
[root@ns01 ~]# firewall-cmd --add-service=dns --permanent
success
[root@ns01 ~]# firewall-cmd --reload
success
[root@ns01 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources:
  services: ssh dhcpv6-client dns
  ports: 
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

[root@ns01 ~]#
```

## Change local resolve.conf
confirm what your local IP is
```bash
[root@ns01 ~]# ip -4 addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    inet 198.18.3.10/24 brd 198.18.3.255 scope global eth0
       valid_lft forever preferred_lft forever
```

And review what your current lookup is set to.  Note that the following is no good.  You want the host to point to itself.  
```bash
[root@ns01 ~]# cat /etc/resolv.conf
# Generated by NetworkManager
search cmed.us
nameserver 8.8.8.8
nameserver 8.8.4.4
[root@ns01 ~]# 
```

Then confirm what your connection name is, and for that connection, overwrite the dns lookup setting
```bash
[root@ns01 ~]# nmcli dev status
DEVICE  TYPE      STATE      CONNECTION
eth0    ethernet  connected  System eth0
lo      loopback  unmanaged  --
[root@ns01 ~]# nmcli con mod System\ eth0 ipv4.dns 198.18.3.10
[root@ns01 ~]# nmcli con up System\ eth0
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/6)
[root@ns01 ~]#
```

Then confirm that the changes were successful
```bash
[root@ns01 ~]# cat /etc/resolv.conf
# Generated by NetworkManager
search cmed.us
nameserver 198.18.3.10
[root@ns01 ~]#
```

## Debugging
### log messages
default logging for bind is in /var/log/messages
```bash
[root@ns01 ~]# cat /var/log/messages | grep named
Jan  4 16:09:36 localhost NetworkManager[664]: <info>  [1515082176.3349] settings: hostname: using hostnamed
Jan  4 17:45:30 localhost systemd-hostnamed: Changed static host name to 'ns01.cmed.us'
Jan  4 17:45:30 localhost systemd-hostnamed: Changed host name to 'ns01.cmed.us'
Jan  4 18:42:35 localhost systemd-hostnamed: Changed pretty host name to 'ns01'
Jan  4 19:44:12 localhost systemd-hostnamed: Changed host name to 'ns01'
Jan  5 16:55:45 localhost bash: /etc/named/zones/db.cmed.us:14: record with inherited owner (cmed.us) immediately after $ORIGIN (cmed.us)
Jan  5 16:55:45 localhost named[23453]: starting BIND 9.9.4-RedHat-9.9.4-51.el7_4.1 -u named -c /etc/named.conf
[...]
Jan  5 16:55:45 localhost named[23453]: all zones loaded
Jan  5 16:55:45 localhost named[23453]: running
[root@ns01 ~]#
```

### Query logging
to enable logging for every query, enter
```bash
[root@ns01 ~]# rndc querylog
[root@ns01 ~]#
```

enter in the same command to turn off this feature. 

running the following dig statement on the bind host
```bash
[root@ns01 ~]# dig fw01.cmed.us @localhost

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.1 <<>> fw01.cmed.us @localhost
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 835
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;fw01.cmed.us. IN A

;; ANSWER SECTION:
fw01.cmed.us. 38400 IN A 198.18.2.1

;; AUTHORITY SECTION:
cmed.us. 38400 IN NS ns01.cmed.us.

;; ADDITIONAL SECTION:
ns01.cmed.us. 38400 IN A 198.18.3.10

;; Query time: 1 msec
;; SERVER: ::1#53(::1)
;; WHEN: Fri Jan 05 18:20:29 GMT 2018
;; MSG SIZE  rcvd: 92

[root@ns01 ~]#
```
produces the following in /var/log/messages
```txt
Jan  5 18:20:29 localhost named[23453]: client ::1#56627 (fw01.cmed.us): query: fw01.cmed.us IN A +E (::1)
```

### Confirming ports
from a remote host, confirm that the ports are up.  (in this case, they are not!)
```bash
Balsa ~ $ nmap 198.18.3.10

Starting Nmap 7.60 ( https://nmap.org ) at 2018-01-05 12:18 EST
Nmap scan report for 198.18.3.10
Host is up (0.0032s latency).
Not shown: 998 closed ports
PORT     STATE    SERVICE
22/tcp   open     ssh
5060/tcp filtered sip

Nmap done: 1 IP address (1 host up) scanned in 1034.67 seconds
Balsa ~ $
```

## References
- [Bind9 on Centos 7](http://net.cmed.us/Home/unixlinux/bind/bind-on-centos7): 
- [How to Setup Bind DNS Server in Chroot Jail on CentOS 7](https://www.ehowstuff.com/bind-dns-server-in-chroot-jail-on-centos-7/): eHowstuff, Skytech, Oct 2014

