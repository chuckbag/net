# Installation of new KVM hosts (with Kimchi)

## Overview: 
This document describes the procedure for installing new KVM hosts with kimchi as the management interface. It was prepared using CentOS 7.8 2003 as the OS and Kimchi version 2.5.0-0. (Aug 10, 2022) 

## Installation 

Install CentOS 7.8 2003 on the KVM host and ensure that the "Virtualization Platform" group of packages is selected.

When the OS is installed and server rebooted, use yum to install the following extra packages. The important one there is kimchi and the rest are all dependencies.

```
nginx-1.16.1-1.el7.ngx.x86_64.rpm
novnc-0.5.1-2.el7.noarch.rpm
python-cheetah-2.4.4-5.el7.centos.x86_64.rpm
python-cherrypy-3.2.2-4.el7.noarch.rpm
python-jsonschema-2.5.1-3.sdl7.noarch.rpm
python-libguestfs-1.40.2-9.el7.x86_64.rpm
python-markdown-2.4.1-1.el7.centos.noarch.rpm
python-psutil-2.2.0-1.gf.el7.x86_64.rpm
python-pygments-1.4-10.el7.noarch.rpm
python-repoze-lru-0.4-3.el7.noarch.rpm
python-websockify-0.6.0-2.el7.noarch.rpm
spice-html5-0.1.7-1.el7.noarch.rpm
wok-2.5.0-0.el7.centos.noarch.rpm
kimchi-2.5.0-0.el7.centos.noarch.rpm
```

Enable `wokd`

Allow incoming traffic to port 8001 in firewalld

```
# firewall-cmd --zone=public --add-port=8001/tcp
# firewall-cmd --zone=public --add-port=8001/udp
```

In `/etc/kimchi/template.conf`, ensure that the "pool" parameter is as follows
```
“pool = images”
```

Reboot the KVM host and when it is back up, the url below should be reachable

https://10.40.35.41:8001/

