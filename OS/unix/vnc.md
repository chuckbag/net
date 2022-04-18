# VNC

- [VNC](#vnc)
  - [Overview](#overview)
  - [Install TigerVNC on the Server](#install-tigervnc-on-the-server)
    - [Install the app](#install-the-app)
    - [Configure the User for the app](#configure-the-user-for-the-app)
    - [Enable the app](#enable-the-app)
    - [Configure the host firewall](#configure-the-host-firewall)
  - [Client](#client)
  - [References](#references)

## Overview 
This is the additional changes you should make on a full install centos7 host, that already has gnome installed on it. 

## Install TigerVNC on the Server
### Install the app

Install tigervnc server
```bash
yum -y install tigervnc-server
```

### Configure the User for the app
This part is a bit weird.  You need to create a different config file for *every* user that you will allow to vnc connect to the server.  

For every user, copy the vnc config file to a new file with a unique "#" in its name (in this case, it's #=1).
```bash
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```

Then edit the config file
```bash
vim /etc/systemd/system/vncserver@:1.service
```

and edit the file and replace the "[Service]" section with the following:
```
[Service]
Type=forking
# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

# user settings for user {username}
ExecStart=/sbin/runuser -l {username} -c "/usr/bin/vncserver %i"
PIDFile=/home/{username}/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
```

where you replace the `{username}` with the users' actual unix username.   

... and you will need to repeat this section for each user, creating a bunch for these configuration files. 

### Enable the app
You will need to also do the following for each of the files (users) you created above. 
```bash
[root@jump01 ~]# systemctl daemon-reload
[root@jump01 ~]# systemctl enable vncserver@:1.service
Created symlink from /etc/systemd/system/multi-user.target.wants/vncserver@:1.service to /etc/systemd/system/vncserver@:1.service.
[root@jump01 ~]#
```

... and you will need to repeat this section for each user, creating a bunch for these configuration files. 

### Configure the host firewall
Make sure that your firewall service is running.  If not, enable it.
```bash
[root@jump01 ~]# firewall-cmd --state
not running
[root@jump01 ~]# systemctl start firewalld
[root@jump01 ~]#
[root@jump01 ~]# firewall-cmd --state
running
[root@jump01 ~]#
```
Allow the ports needed for VNC into the server. 
```bash
[root@jump01 ~]# firewall-cmd --permanent --zone=public --add-service vnc-server
success
[root@jump01 ~]# firewall-cmd --reload
success
[root@jump01 ~]#
[root@jump01 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens192
  sources:
  services: ssh dhcpv6-client vnc-server
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

[root@jump01 ~]#
```


## Client
How to install a client, and how the client can use VNC to connect to the server.  
- VNC Windows Client: Connecting in from a windows computer
- VNC Mac Client: Connecting in from a Mac computer


## References
- [How To Install and Configure VNC Remote Access for the GNOME Desktop on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-remote-access-for-the-gnome-desktop-on-centos-7): Digital Ocean, Nov 2014
- [VNC-Server installation on CentOS 7](https://www.howtoforge.com/vnc-server-installation-on-centos-7): HowtoForge, Srijan Kishore 
