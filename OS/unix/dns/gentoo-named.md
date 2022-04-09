# Gentoo named

- [Gentoo named](#gentoo-named)
  - [Bind on Gentoo](#bind-on-gentoo)
  - [Setting up Bind](#setting-up-bind)
    - [Install Bind](#install-bind)
    - [Configuring Bind on Gentoo](#configuring-bind-on-gentoo)
  - [Configuring Bind](#configuring-bind)
    - [The Files in Bind](#the-files-in-bind)
    - [The /etc/named.conf File](#the-etcnamedconf-file)
  - [Starting and Stopping BIND](#starting-and-stopping-bind)
  - [Troubleshooting Bind/DNS](#troubleshooting-binddns)
  - [References](#references)

## Bind on Gentoo
This doc goes through the steps to take to make a gentoo host run bind.

## Setting up Bind
These steps go through what it will take to get bind working on a gentoo host. There are some things that are specific to gentoo, and then there are a few things that might be different to you that are part of the standard gentoo install. This section covers these steps.

### Install Bind
Gentoo bind comes setup in a chroot jail. This allows the setup to be more secure, by preventing a hacker who exploits bind from gaining higher level access to the server. The linux security website has a bunch of good notes on chroot-ing bind.

Installing and running bind

check to see if you have bind installed
```bash
root@host # emerge -s ^bind$ bind-tools
Searching...   
[ Results for search key : ^bind$ ]
[ Applications found : 1 ]
 
*  net-dns/bind
      Latest version available: 9.2.2-r2
      Latest version installed: 
      Size of downloaded files: 4,936 kB
      Homepage:    http://www.isc.org/products/BIND/bind9.html
      Description: BIND - Berkeley Internet Name Domain - Name Server


Searching...   
[ Results for search key : bind-tools ]
[ Applications found : 1 ]
 
*  net-dns/bind-tools
      Latest version available: 9.2.2
      Latest version installed: 
      Size of downloaded files: 4,951 kB
      Homepage:    http://www.isc.org/products/BIND/bind9-beta.html
      Description: bind tools: dig, nslookup, and host
```

install bind with dns tools
```bash
root@host # emerge bind; emerge bind-tools
(bla...bla...bla)
```

change bind so that it works within a chrooted environment:
```bash
root@host # ebuild /var/db/pkg/net-dns/bind-9.2.2-r2/bind-9.2.2-r2.ebuild config
```

continuing with this, edit roots .bashrc file
```bash
root@host # vim .bashrc
	! -- add this to the file
	alias rndc='rndc -k /chroot/dns/etc/bind/rndc.key'
```

then enable the changes to the .bashrc file
```bash
root@host # source /root/.bashrc
```

### Configuring Bind on Gentoo
Since this bind configuration is setup in a chrooted environment, the files that you will need to modify are in locations that are different than the standard install. In this section, we are only going over where files will be located within gentoo. The following section will cover what those files are, and how they interact with each other.

You can find the primary chrooted bind configuration file in the following location:
```
/chroot/dns/etc/bind/named.conf
```
From here, you can define the forward and reverse files, which would be here:
```
/chroot/dns/var/bind/pri/db.cmed.us
/chroot/dns/var/bind/pri/db.192.168.1
```

The log file for bind is located here:
```
/var/log/named/current
```
-or-
```
/var/log/everything/current
```

gentoo config for named:
```
/etc/conf.d/named
```

## Configuring Bind
This section will cover the steps that are standard to all bind installs.

### The Files in Bind
To make a correctly installed version of BIND work, you need to setup four different types of files. These configuration files tells BIND how it is configured, and all the information about all the computers and their corresponding ip addresses.

The table below lists the primary files in play on a bind setup. Note that in a chrooted configuration, the file locations are different than listed below.

|File	|Description|
|--|--|
`/etc/named.conf` | This is the master config file. This keeps track of where all the other files are kept, as well as defining what domains that BIND is handling, and across what networks.
`db.ADDR` | This file is also know as the forward lookup file. There is one of these files per domain or subdomain. Basically this file specify's the domain, like people.org, and lists all the hosts and ips, like Jack = 10.1.1.1. It is used for all forward lookups, thus if you want to resolve the ip for the name Joan.people.org, this file would tell you that it is 10.1.2.2. The location of this file is specified in the `/etc/named.conf` file.
`db.REVERSE` | The Reverse lookup file handles all "reverse" queries. That would be lookups where you know the IP but not the name. Thus, if you wanted to find out who is 10.1.2.4, you could do a reverse lookup using this file and see that it is Meg. The location of this file is specified in the `/etc/named.conf` file.
`db.127.0.0` | The loopback file is simply a small file that tells BIND to ask itself if it needs to look something up. Since the computer itself doesn't point to a name server (like when it is configured), all queries need to be asked to someone, and this file reminds the host that it can ask itself and get the answer. The location of this file is specified in the `/etc/named.conf` file.
`db.cache` | The cache file, otherwise known as the "." file, keeps track of all the DNS root servers. These servers know all the domains on the internet, so if you can not resolve an outside address, one of the roots will at least get you to someone that can. The location of this file is specified in the `/etc/named.conf` file.

Of the above files, we will go over a few of them below.

### The /etc/named.conf File
The primary file is the `/etc/named.conf` file. This is the central configuration file that organizes where everything else is kept. This file is actually really basic, with only a few different types of things in it. It defines what everything else is, and where it's kept. Not much more than that. (Remember that this is a basic example, it can have more goodies added, but we won't mess with that in this discussion.)

There are only two different things that this file defines, the first is options which is defined in the header, and the second is zones, which takes up the body of the document. The options are global statements, like where the rest of the files for BIND are kept in the unix directory, and things like that. The zone files define all the different domains and what files contain what info.

Here's the example /etc/named.conf file for the people.org network:

The /etc/named.conf File
```txt
01  ! -- 
// BIND configuration file	
options {
        directory "/var/bind";
        listen-on-v6 { none; };
        listen-on { 127.0.0.1; 192.168.1.23; };

        pid-file "/var/run/named/named.pid";
};

zone "." IN {
        type hint;
        file "named.ca";
};

zone "localhost" IN {
        type master;
        file "pri/localhost.zone";
        allow-update { none; };
        notify no;
};

zone "127.in-addr.arpa" IN {
        type master;
        file "pri/127.zone";
        allow-update { none; };
        notify no;
};

zone "cmed.us" {
        type master; 
        file "pri/db.cmed.us";
};
```

## Starting and Stopping BIND
How to Startup Bind

start bind
```bash
root@host # /etc/init.d/named start
```

viewing the named process
```bash
slh bind # ps -ef|grep named
named    21683     1  0 19:45 ?        00:00:00 [named]
named    21684 21683  0 19:45 ?        00:00:00 [named]
named    21686 21684  0 19:45 ?        00:00:00 [named]
named    21687 21684  0 19:45 ?        00:00:00 [named]
named    21688 21684  0 19:45 ?        00:00:00 [named]
```

set bind to run as default
```bash
root@host # rc-update add named default
```
	
##  Troubleshooting Bind/DNS

## References
- [Linux Security; Chroot Jail](http://www.linuxsecurity.com/docs/LDP/Chroot-BIND-HOWTO-2.html). Describes how bind can be installed (and what is) a chroot jail.
- [DNS and BIND, 4th Edition](http://www.oreilly.com/catalog/dns4/index.html). By Paul Albitz, Cricket Liu 4th Edition April 2001 © O'Reilly & Associates, Inc.
- [DNS & BIND Cookbook](http://www.oreilly.com/catalog/dnsbindckbk/index.html). By Cricket Liu October 2002 © O'Reilly & Associates, Inc.
