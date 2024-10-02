# Switching a host around

- [Switching a host around](#switching-a-host-around)
  - [Changing the hostname](#changing-the-hostname)
  - [Changing the IP/Static/DHCP](#changing-the-ipstaticdhcp)
    - [Connection names](#connection-names)
    - [All interface info](#all-interface-info)
    - [Set Static IP](#set-static-ip)
    - [Hosing an interface](#hosing-an-interface)
  - [Changing DNS](#changing-dns)
  - [References](#references)

## Changing the hostname
The CentOS 7 method for changing the hostname is though the hostnamectl command.  Once you have made the change, you can confirm by just running the command alone again.  (note that you will need to re-login to have your shell name take affect.)  
```bash
[root@localhost ~]# hostnamectl set-hostname ns01.cmed.us
[root@localhost ~]# hostnamectl
   Static hostname: ns01.cmed.us
         Icon name: computer-vm
           Chassis: vm
        Machine ID: f94a5615c152494fa544014fa37efed6
           Boot ID: 3199871f89bd4d148dc124d0b71c02a8
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-693.el7.x86_64
      Architecture: x86-64
[root@localhost ~]#
```

The problem with doing this is that the hostname command only provides the fqdn, rather then *just* the hostname. Note that the system still understands the domain name is "cmed.us" (-d), but it can't provide the non-fqdn name.  
```bash
[root@localhost ~]# hostname
ns01.cmed.us
[root@localhost ~]# hostname -f
ns01.cmed.us
[root@localhost ~]# hostname -d
cmed.us
[root@localhost ~]#
```

## Changing the IP/Static/DHCP

### Connection names
First, figure out what all the interfaces are and what their connection names are.  You'll need to know the connection names to make changes. 

You can do this either with the connection show, 
```bash
[root@localhost ~]# nmcli con show
NAME         UUID                                  TYPE            DEVICE
System eth0  6e8bfaad-741a-47fa-9ee1-7d46c18fb616  802-3-ethernet  eth0
[root@localhost ~]# 
```

or device status commands
```bash
[root@localhost ~]# nmcli dev status
DEVICE  TYPE      STATE      CONNECTION
eth0    ethernet  connected  System eth0
lo      loopback  unmanaged  --
[root@localhost ~]#
```

### All interface info
Use the connection name info, to see all the info about a specific interface.  
```bash
[root@localhost ~]# nmcli con show System\ eth0
connection.id:                          System eth0
connection.uuid:                        6e8bfaad-741a-47fa-9ee1-7d46c18fb616
connection.stable-id:                   --
connection.interface-name:              eth0
connection.type:                        802-3-ethernet
connection.autoconnect:                 yes
connection.autoconnect-priority:        0
connection.autoconnect-retries:         -1 (default)
connection.timestamp:                   1515096576
connection.read-only:                   no
connection.permissions:                 --
connection.zone:                        --
connection.master:                      --
connection.slave-type:                  --
connection.autoconnect-slaves:          -1 (default)
connection.secondaries:                 --
connection.gateway-ping-timeout:        0
connection.metered:                     unknown
connection.lldp:                        -1 (default)
802-3-ethernet.port:                    --
802-3-ethernet.speed:                   0
802-3-ethernet.duplex:                  --
802-3-ethernet.auto-negotiate:          no
802-3-ethernet.mac-address:             --
802-3-ethernet.cloned-mac-address:      --
802-3-ethernet.generate-mac-address-mask:--
802-3-ethernet.mac-address-blacklist:   --
802-3-ethernet.mtu:                     auto
802-3-ethernet.s390-subchannels:        --
802-3-ethernet.s390-nettype:            --
802-3-ethernet.s390-options:            --
802-3-ethernet.wake-on-lan:             1 (default)
802-3-ethernet.wake-on-lan-password:    --
ipv4.method:                            auto
ipv4.dns:                               --
ipv4.dns-search:                        --
ipv4.dns-options:                       (default)
ipv4.dns-priority:                      0
ipv4.addresses:                         --
ipv4.gateway:                           --
ipv4.routes:                            --
ipv4.route-metric:                      -1
ipv4.ignore-auto-routes:                no
ipv4.ignore-auto-dns:                   no
ipv4.dhcp-client-id:                    --
ipv4.dhcp-timeout:                      0
ipv4.dhcp-send-hostname:                yes
ipv4.dhcp-hostname:                     --
ipv4.dhcp-fqdn:                         --
ipv4.never-default:                     no
ipv4.may-fail:                          yes
ipv4.dad-timeout:                       -1 (default)
ipv6.method:                            ignore
ipv6.dns:                               --
ipv6.dns-search:                        --
ipv6.dns-options:                       (default)
ipv6.dns-priority:                      0
ipv6.addresses:                         --
ipv6.gateway:                           --
ipv6.routes:                            --
ipv6.route-metric:                      -1
ipv6.ignore-auto-routes:                no
ipv6.ignore-auto-dns:                   no
ipv6.never-default:                     no
ipv6.may-fail:                          yes
ipv6.ip6-privacy:                       -1 (unknown)
ipv6.addr-gen-mode:                     stable-privacy
ipv6.dhcp-send-hostname:                yes
ipv6.dhcp-hostname:                     --
ipv6.token:                             --
proxy.method:                           none
proxy.browser-only:                     no
proxy.pac-url:                          --
proxy.pac-script:                       --
GENERAL.NAME:                           System eth0
GENERAL.UUID:                           6e8bfaad-741a-47fa-9ee1-7d46c18fb616
GENERAL.DEVICES:                        eth0
GENERAL.STATE:                          activated
GENERAL.DEFAULT:                        yes
GENERAL.DEFAULT6:                       no
GENERAL.VPN:                            no
GENERAL.ZONE:                           --
GENERAL.DBUS-PATH:                      /org/freedesktop/NetworkManager/ActiveConnection/2
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/Settings/1
GENERAL.SPEC-OBJECT:                    --
GENERAL.MASTER-PATH:                    --
IP4.ADDRESS[1]:                         198.18.3.212/24
IP4.GATEWAY:                            198.18.3.1
IP4.DNS[1]:                             8.8.8.8
IP4.DNS[2]:                             8.8.4.4
IP4.DOMAIN[1]:                          cmed.us
DHCP4.OPTION[1]:                        requested_classless_static_routes = 1
DHCP4.OPTION[2]:                        requested_rfc3442_classless_static_routes = 1
DHCP4.OPTION[3]:                        subnet_mask = 255.255.255.0
DHCP4.OPTION[4]:                        server_name = pxeserver
DHCP4.OPTION[5]:                        domain_name_servers = 8.8.8.8 8.8.4.4
DHCP4.OPTION[6]:                        requested_subnet_mask = 1
DHCP4.OPTION[7]:                        ip_address = 198.18.3.212
DHCP4.OPTION[8]:                        filename = pxelinux.0
DHCP4.OPTION[9]:                        requested_static_routes = 1
DHCP4.OPTION[10]:                       dhcp_server_identifier = 198.18.3.20
DHCP4.OPTION[11]:                       requested_nis_servers = 1
DHCP4.OPTION[12]:                       requested_time_offset = 1
DHCP4.OPTION[13]:                       broadcast_address = 198.18.3.255
DHCP4.OPTION[14]:                       requested_interface_mtu = 1
DHCP4.OPTION[15]:                       dhcp_rebinding_time = 3012
DHCP4.OPTION[16]:                       requested_domain_name_servers = 1
DHCP4.OPTION[17]:                       dhcp_message_type = 5
DHCP4.OPTION[18]:                       requested_broadcast_address = 1
DHCP4.OPTION[19]:                       routers = 198.18.3.1
DHCP4.OPTION[20]:                       dhcp_renewal_time = 1662
DHCP4.OPTION[21]:                       requested_domain_name = 1
DHCP4.OPTION[22]:                       domain_name = cmed.us
DHCP4.OPTION[23]:                       requested_routers = 1
DHCP4.OPTION[24]:                       expiry = 1515099112
DHCP4.OPTION[25]:                       requested_wpad = 1
DHCP4.OPTION[26]:                       host_name = ns01
DHCP4.OPTION[27]:                       requested_nis_domain = 1
DHCP4.OPTION[28]:                       requested_ms_classless_static_routes = 1
DHCP4.OPTION[29]:                       network_number = 198.18.3.0
DHCP4.OPTION[30]:                       requested_domain_search = 1
DHCP4.OPTION[31]:                       next_server = 198.18.3.20
DHCP4.OPTION[32]:                       requested_ntp_servers = 1
DHCP4.OPTION[33]:                       ntp_servers = 199.223.248.99
DHCP4.OPTION[34]:                       dhcp_lease_time = 3600
DHCP4.OPTION[35]:                       requested_host_name = 1
IP6.ADDRESS[1]:                         fe80::5054:ff:fe93:d50/64
IP6.GATEWAY:                            --
[root@localhost ~]#
```

By looking at the device (rather then the connection name), we see a bunch of good things too. 
```bash
[root@localhost ~]# nmcli dev show eth0
GENERAL.DEVICE:                         eth0
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         52:54:00:93:0D:50
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     System eth0
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/2
WIRED-PROPERTIES.CARRIER:               on
IP4.ADDRESS[1]:                         198.18.3.212/24
IP4.GATEWAY:                            198.18.3.1
IP4.DNS[1]:                             8.8.8.8
IP4.DNS[2]:                             8.8.4.4
IP4.DOMAIN[1]:                          cmed.us
IP6.ADDRESS[1]:                         fe80::5054:ff:fe93:d50/64
IP6.GATEWAY:                            --
[root@localhost ~]#
```

### Set Static IP
Set the address, gateway and method (manual (aka static) or auto (dhcp))
```bash
[root@localhost ~]# nmcli con mod System\ eth0 ipv4.address 198.18.3.10/24
[root@localhost ~]# nmcli con mod System\ eth0 ipv4.gateway 198.18.3.1
[root@localhost ~]# nmcli con mod System\ eth0 ipv4.method manual
[root@localhost ~]# nmcli con up System\ eth0
```

### Hosing an interface
if you want to shut down an interface, or prevent it from auto connecting after a reboot do the following: 
```bash
[root@localhost ~]# nmcli con down System\ eth0
[root@localhost ~]# nmcli con show
NAME         UUID                                  TYPE            DEVICE
System eth0  6e8bfaad-741a-47fa-9ee1-7d46c18fb616  802-3-ethernet  --    
[root@localhost ~]#
```

```bash
[root@localhost ~]# nmcli con up System\ eth
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/1)
```

```bash
[root@localhost ~]# nmcli con mod System\ eth connection.autoconnect no
```


## Changing DNS
Replace dns with ipv4.dns, add with + and remove wit -
```bash
[root@ns01 ~]# nmcli con mod System\ eth0 ipv4.dns 8.8.8.8
[root@ns01 ~]# nmcli con mod System\ eth0 +ipv4.dns 8.8.4.4
[root@ns01 ~]# nmcli con mod System\ eth0 +ipv4.dns-search cmed.us
[root@ns01 ~]# nmcli con up System\ eth0
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)
[root@ns01 ~]#
```

Confirm the changes took: 
```bash
[root@ns01 ~]# cat /etc/resolv.conf
# Generated by NetworkManager
search cmed.us
nameserver 8.8.8.8
nameserver 8.8.4.4
[root@ns01 ~]#
```



## References
- [How to set hostname and FQDN on CentOS 7 and RHEL 7](http://sharadchhetri.com/2014/09/27/set-hostname-fqdn-centos-7-rhel-7/): Sharad Chhetri, Sep 2014
- [RHEL7: Configure IPv4 addresses and perform basic IPv4 troubleshooting](https://www.certdepot.net/rhel7-configure-ipv4-addresses/): CertDepot, Nov 2017

Just as a side reference, the results for a connection show for a static assigned interface would look like such: 
```bash
[root@ns01 ~]# nmcli con show System\ eth0
connection.id:                          System eth0
connection.uuid:                        6e8bfaad-741a-47fa-9ee1-7d46c18fb616
connection.stable-id:                   --
connection.interface-name:              eth0
connection.type:                        802-3-ethernet
connection.autoconnect:                 yes
connection.autoconnect-priority:        0
connection.autoconnect-retries:         -1 (default)
connection.timestamp:                   1515100476
connection.read-only:                   no
connection.permissions:                 --
connection.zone:                        --
connection.master:                      --
connection.slave-type:                  --
connection.autoconnect-slaves:          -1 (default)
connection.secondaries:                 --
connection.gateway-ping-timeout:        0
connection.metered:                     unknown
connection.lldp:                        -1 (default)
802-3-ethernet.port:                    --
802-3-ethernet.speed:                   0
802-3-ethernet.duplex:                  --
802-3-ethernet.auto-negotiate:          no
802-3-ethernet.mac-address:             --
802-3-ethernet.cloned-mac-address:      --
802-3-ethernet.generate-mac-address-mask:--
802-3-ethernet.mac-address-blacklist:   --
802-3-ethernet.mtu:                     auto
802-3-ethernet.s390-subchannels:        --
802-3-ethernet.s390-nettype:            --
802-3-ethernet.s390-options:            --
802-3-ethernet.wake-on-lan:             1 (default)
802-3-ethernet.wake-on-lan-password:    --
ipv4.method:                            manual
ipv4.dns:                               8.8.8.8,8.8.4.4
ipv4.dns-search:                        cmed.us
ipv4.dns-options:                       (default)
ipv4.dns-priority:                      0
ipv4.addresses:                         198.18.3.10/24
ipv4.gateway:                           198.18.3.1
ipv4.routes:                            --
ipv4.route-metric:                      -1
ipv4.ignore-auto-routes:                no
ipv4.ignore-auto-dns:                   no
ipv4.dhcp-client-id:                    --
ipv4.dhcp-timeout:                      0
ipv4.dhcp-send-hostname:                yes
ipv4.dhcp-hostname:                     --
ipv4.dhcp-fqdn:                         --
ipv4.never-default:                     no
ipv4.may-fail:                          yes
ipv4.dad-timeout:                       -1 (default)
ipv6.method:                            ignore
ipv6.dns:                               --
ipv6.dns-search:                        --
ipv6.dns-options:                       (default)
ipv6.dns-priority:                      0
ipv6.addresses:                         --
ipv6.gateway:                           --
ipv6.routes:                            --
ipv6.route-metric:                      -1
ipv6.ignore-auto-routes:                no
ipv6.ignore-auto-dns:                   no
ipv6.never-default:                     no
ipv6.may-fail:                          yes
ipv6.ip6-privacy:                       -1 (unknown)
ipv6.addr-gen-mode:                     stable-privacy
ipv6.dhcp-send-hostname:                yes
ipv6.dhcp-hostname:                     --
ipv6.token:                             --
proxy.method:                           none
proxy.browser-only:                     no
proxy.pac-url:                          --
proxy.pac-script:                       --
GENERAL.NAME:                           System eth0
GENERAL.UUID:                           6e8bfaad-741a-47fa-9ee1-7d46c18fb616
GENERAL.DEVICES:                        eth0
GENERAL.STATE:                          activated
GENERAL.DEFAULT:                        yes
GENERAL.DEFAULT6:                       no
GENERAL.VPN:                            no
GENERAL.ZONE:                           --
GENERAL.DBUS-PATH:                      /org/freedesktop/NetworkManager/ActiveConnection/5
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/Settings/1
GENERAL.SPEC-OBJECT:                    --
GENERAL.MASTER-PATH:                    --
IP4.ADDRESS[1]:                         198.18.3.10/24
IP4.GATEWAY:                            198.18.3.1
IP4.DNS[1]:                             8.8.8.8
IP4.DNS[2]:                             8.8.4.4
IP6.ADDRESS[1]:                         fe80::5054:ff:fe93:d50/64
IP6.GATEWAY:                            --
[root@ns01 ~]#
```
