# network interfaces

## Overview

## ipconfig
Microsofts version of ifconfig.  If you want to only view the first couple of lines, pipe the output to more.  IE: `ipconfig | more`
```
PS C:\Users\cmercier> ipconfig

Windows IP Configuration


Ethernet adapter TAP:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Wireless LAN adapter wi0:

   Connection-specific DNS Suffix  . : cmed.us
   Link-local IPv6 Address . . . . . : fe80::f585:8d36:5ba5:68dd%15
   IPv4 Address. . . . . . . . . . . : 192.168.100.123
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.100.1

Ethernet adapter eth0:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Ethernet adapter VMware Network Adapter VMnet1:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::7d28:257b:a23c:8705%25
   IPv4 Address. . . . . . . . . . . : 192.168.237.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :

Ethernet adapter VMware Network Adapter VMnet8:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::80f5:6aa7:210b:1842%26
   IPv4 Address. . . . . . . . . . . : 192.168.192.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :

Tunnel adapter isatap.{70A77F18-9322-4F63-BA69-740FD0ADE33F}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Tunnel adapter isatap.{322C7565-0BED-43FF-A0EB-7B4F57FB87C3}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Tunnel adapter isatap.{38C1339B-00BD-4BC8-B3C8-68FDBE8B94ED}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Tunnel adapter Teredo Tunneling Pseudo-Interface:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Tunnel adapter isatap.{F3C8B2CA-4986-4A09-AC22-E93C582555CB}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Tunnel adapter isatap.credorax.com:

   Connection-specific DNS Suffix  . : cmed.us
   Link-local IPv6 Address . . . . . : fe80::5efe:192.168.100.123%29
   Default Gateway . . . . . . . . . :
PS C:\Users\cmercier>
```

## netsh
better for outputting more specific output.
```
PS C:\Users\cmercier> netsh interface ip show address eth0

Configuration for interface "eth0"
    DHCP enabled:                         Yes
    IP Address:                           10.33.192.32
    Subnet Prefix:                        10.33.192.0/24 (mask 255.255.255.0)
    Default Gateway:                      10.33.192.1
    Gateway Metric:                       0
    InterfaceMetric:                      10

PS C:\Users\cmercier> netsh int ip sho address eth0

Configuration for interface "eth0"
    DHCP enabled:                         Yes
    IP Address:                           10.33.192.32
    Subnet Prefix:                        10.33.192.0/24 (mask 255.255.255.0)
    Default Gateway:                      10.33.192.1
    Gateway Metric:                       0
    InterfaceMetric:                      10

PS C:\Users\cmercier> netsh int ip sho address wi0

Configuration for interface "wi0"
    DHCP enabled:                         Yes
    IP Address:                           192.168.100.123
    Subnet Prefix:                        192.168.100.0/24 (mask 255.255.255.0)
    Default Gateway:                      192.168.100.1
    Gateway Metric:                       0
    InterfaceMetric:                      25

PS C:\Users\cmercier>
```
