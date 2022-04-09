# centos named
- [centos named](#centos-named)
  - [Installing:](#installing)
  - [Overview:](#overview)
    - [Where:](#where)
    - [What:](#what)
    - [How:](#how)
  - [Setting Up:](#setting-up)
    - [resolv.conf](#resolvconf)
    - [named.conf:](#namedconf)
    - [db.ADDR Example](#dbaddr-example)
    - [db.REVERSE](#dbreverse)
  - [References:](#references)
  
## Installing:
Install Bind and the DNS utilities:
```bash
# yum install bind.i386
# yum install bind-utils.i386
```

## Overview:
### Where:
Everything is chrooted for security.  All the files will go in this directory:
```bash
# cd /var/named/chroot/var/named/
```

### What:
In Bind, there are a couple of files to keep track of:
1. named.conf: This lists what all the other files are named
2. "db.ADDR":  The forward name space or the primary name file.  It won't be labeled "db.ADDR", instead the "ADDR" will be replaced with your domain name, ie: "db.cmed"
3. "db.REVERSE": which lists the IP's and what name should be associated to them.  There will be one per class "C" network, and one for link local.

You will need to create these files, and you will place them in the following locations.
```txt
/var/named/chroot/etc/named.conf
/var/named/chroot/var/named/db.cmed
/var/named/chroot/var/named/db.198.18.2
/var/named/chroot/var/named/db.198.18.1
/var/named/chroot/var/named/db.198.18.0
/var/named/chroot/var/named/db.127.0.0
```

### How:
Starting and stopping bind in centos is very simply, simply enter the following commands:
```bash
service named start|stop|status|restart
```

The log files can be found in the following directories:

??????????????

## Setting Up:
### resolv.conf
You are going to have to create your own config and db files to get going, but first, lets make sure this computer knows it's a name server.
```bash
vim /etc/resolv.conf
```
Tell the local resolver to resolve "nameserver" to the IP address of the server you are setting up. 

nameserver 198.18.2.31

### named.conf:
The named.conf file is the main "table of contents" file that points to all of the other files and where they are located. 

You can view the default example file in
```bash
/usr/share/doc/bind-9.x.x/sample/etc/named.conf
```

and if you want to start with this and modify it, you could always copy it to the correct location and edit it there. 
```bash
cp /usr/share/doc/bind-9.x.x/sample/etc/named.conf /var/named/chroot/etc/named.conf
```

For reference, we have two named.conf files to refer to:
- [sample named.conf (for centos + bind 9.3.6)](named.conf-example-bind9.6.txt)
- [my named.conf file](named.conf-cmed.us.txt)



### db.ADDR Example
The following is an example of the db.cmed file:
```bash
vim /var/named/chroot/var/named/db.cmed
```

```txt
$ORIGIN cmed.us.         ; designates the start of this zone file in the name space
$TTL 1h                  ; default expiration time of all resource records without their own TTL value
cmed.us.  IN  SOA  ns.cmed.us. username.cmed.us. (
              2010121300 ; serial number of this zone file
              1d         ; slave refresh (1 day)
              2h         ; slave retry time in case of a problem (2 hours)
              4w         ; slave expiration time (4 weeks)
              1h         ; minimum caching time in case of failed lookups (1 hour)
              )
;
;   Name servers 
;
cmed.us.      NS        ns                    ; ns.cmed.us is a nameserver for cmed.us

;
;   Mail servers
;
cmed.us.      MX        10 mail.cmed.us.      ; mail.cmed.us is the mailserver for cmed.us


; ### ---------- Public, v10 ------------ ### 
NETWORK.v10   A         198.18.0.0 
demark        A         198.18.0.1

www.v10       A         198.18.0.30

dhcp10.0      A         198.18.0.100
dhcp10.1      A         198.18.0.101
dhcp10.2      A         198.18.0.102
dhcp10.3      A         198.18.0.103
dhcp10.4      A         198.18.0.104
dhcp10.5      A         198.18.0.105
dhcp10.6      A         198.18.0.106
dhcp10.7      A         198.18.0.107
dhcp10.8      A         198.18.0.108
dhcp10.9      A         198.18.0.109
dhcp10.10     A         198.18.0.110
dhcp10.11     A         198.18.0.111
dhcp10.12     A         198.18.0.112
dhcp10.13     A         198.18.0.113
dhcp10.14     A         198.18.0.114
dhcp10.15     A         198.18.0.115
dhcp10.16     A         198.18.0.116
dhcp10.17     A         198.18.0.117
dhcp10.18     A         198.18.0.118
dhcp10.19     A         198.18.0.119
dhcp10.20     A         198.18.0.120
dhcp10.21     A         198.18.0.121
dhcp10.22     A         198.18.0.122
dhcp10.23     A         198.18.0.123
dhcp10.24     A         198.18.0.124
dhcp10.25     A         198.18.0.125
dhcp10.26     A         198.18.0.126
dhcp10.27     A         198.18.0.127
dhcp10.28     A         198.18.0.128
dhcp10.29     A         198.18.0.129

pix.v10       A         198.18.0.254
BROADCAST.v10 A         198.18.0.255

; ### ---------- DMZ, v11 ------------ ### 
NETWORK.v11   A         198.18.1.0 
pix.v11       A         198.18.1.1

gallery       A         198.18.1.30

BROADCAST.v11 A         198.18.1.255

; ### ---------- private, v12 ------------ ### 
; .1-.9       = defroutes
NETWORK.v12   A         198.18.2.0 
pix.v12       A         198.18.2.1
pix           cname     pix.v12
defroute      cname     pix.v12

; .10-.19     = appliances
printer       A         198.18.2.15
squeezebox    A         198.18.2.16
wii           A         198.18.2.17

; .30-.39     = servers
taco          A         198.18.2.30
filer         cname     taco

brie          A         198.18.2.31
mail          cname     brie
ns            cname     brie

; .50-.59     = static desktops
kitchen       A         198.18.2.50

; .100-.129   = dhcp pool
dhcp11.0      A         198.18.2.100
dhcp11.1      A         198.18.2.101
dhcp11.2      A         198.18.2.102
dhcp11.3      A         198.18.2.103
dhcp11.4      A         198.18.2.104
dhcp11.5      A         198.18.2.105
dhcp11.6      A         198.18.2.106
dhcp11.7      A         198.18.2.107
dhcp11.8      A         198.18.2.108
dhcp11.9      A         198.18.2.109
dhcp11.10     A         198.18.2.110
dhcp11.11     A         198.18.2.111
dhcp11.12     A         198.18.2.112
dhcp11.13     A         198.18.2.113
dhcp11.14     A         198.18.2.114
dhcp11.15     A         198.18.2.115
dhcp11.16     A         198.18.2.116
dhcp11.17     A         198.18.2.117
dhcp11.18     A         198.18.2.118
dhcp11.19     A         198.18.2.119
dhcp11.20     A         198.18.2.120
dhcp11.21     A         198.18.2.121
dhcp11.22     A         198.18.2.122
dhcp11.23     A         198.18.2.123
dhcp11.24     A         198.18.2.124
dhcp11.25     A         198.18.2.125
dhcp11.26     A         198.18.2.126
dhcp11.27     A         198.18.2.127
dhcp11.28     A         198.18.2.128
dhcp11.29     A         198.18.2.129

; .240-.249   = switches
zugzug        A         198.18.2.240
switch        A         198.18.2.245

BROADCAST.v11 A         198.18.2.255
```

### db.REVERSE

## References:
- [How to make Bind Work in CENTOS 5.x version?](http://portal.hostingcontroller.com/KB/a122/how-to-make-bind-work-in-centos-5x-version.aspx)

