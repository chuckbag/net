# PIX Version 4.4(5) 

```
pixfirewall# wr term
Building configuration...
: Saved
:
PIX Version 4.4(5)
nameif ethernet0 outside security0
nameif ethernet1 inside security100
nameif ethernet2 pix/intf2 security10
nameif ethernet3 pix/intf3 security15
nameif ethernet4 pix/intf4 security20
nameif ethernet5 pix/intf5 security25
enable password 8Ry2YjIyt7RRXU24 encrypted
passwd 2KFQnbNIdI.2KYOU encrypted
hostname pixfirewall
fixup protocol ftp 21
fixup protocol http 80
fixup protocol h323 1720
fixup protocol rsh 514
fixup protocol smtp 25
fixup protocol sqlnet 1521
names
pager lines 24
logging on
no logging timestamp
no logging console
no logging monitor
no logging buffered
no logging trap
logging facility 20
logging queue 512
interface ethernet0 auto
interface ethernet1 auto
interface ethernet2 auto
interface ethernet3 auto
interface ethernet4 auto
interface ethernet5 auto
mtu outside 1500
mtu inside 1500
mtu pix/intf2 1500
mtu pix/intf3 1500
mtu pix/intf4 1500
mtu pix/intf5 1500
ip address outside 127.0.0.1 255.255.255.255
ip address inside 127.0.0.1 255.255.255.255
ip address pix/intf2 127.0.0.1 255.255.255.255
ip address pix/intf3 127.0.0.1 255.255.255.255
ip address pix/intf4 127.0.0.1 255.255.255.255
ip address pix/intf5 127.0.0.1 255.255.255.255
no failover
failover timeout 0:00:00
failover ip address outside 0.0.0.0
failover ip address inside 0.0.0.0
failover ip address pix/intf2 0.0.0.0
failover ip address pix/intf3 0.0.0.0
failover ip address pix/intf4 0.0.0.0
failover ip address pix/intf5 0.0.0.0
arp timeout 14400
no rip outside passive
no rip outside default
no rip inside passive
no rip inside default
no rip pix/intf2 passive
no rip pix/intf2 default
no rip pix/intf3 passive
no rip pix/intf3 default
no rip pix/intf4 passive
no rip pix/intf4 default
no rip pix/intf5 passive
no rip pix/intf5 default
timeout xlate 3:00:00 conn 1:00:00 half-closed 0:10:00 udp 0:02:00
timeout rpc 0:10:00 h323 0:05:00
timeout uauth 0:05:00 absolute
aaa-server TACACS+ protocol tacacs+
aaa-server RADIUS protocol radius
no snmp-server location
no snmp-server contact
snmp-server community public
no snmp-server enable traps
telnet timeout 5
terminal width 80
Cryptochecksum:d41d8cd98f00b204e9800998ecf8427e
: end
[OK]
```