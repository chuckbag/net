# Advanced Troubleshooting

- [Advanced Troubleshooting](#advanced-troubleshooting)
  - [ssh to AP:](#ssh-to-ap)
  - [From the CLI:](#from-the-cli)
    - [System Info:](#system-info)
    - [Ping:](#ping)
  - [From The Linux Shell](#from-the-linux-shell)
    - [Break out of CLI:](#break-out-of-cli)
    - [tcpdump lan interface:](#tcpdump-lan-interface)
    - [remotely log into the other AP:](#remotely-log-into-the-other-ap)
    - [Enable Discovery Agent:](#enable-discovery-agent)
    - [log out of remote APs](#log-out-of-remote-aps)
  - [Check the local Zone Director:](#check-the-local-zone-director)
    - [Define Zone Director:](#define-zone-director)


## ssh to AP: 
use your standard admin account user/pass: 
```
➤ ssh admin@198.18.2.242
Please login: admin
Password:
ruckus> ena
ruckus#
```

## From the CLI: 

### System Info: 
see the unit's basic info
```
ruckus# sho sysinfo
System Overview:
  Name= Ruckus-Unleashed
  IP Address= 198.18.2.242
  MAC Address= 58:B6:33:06:61:E0
  Uptime= 15h 59m
  Model= R500
  Licensed APs= 25
  Serial Number= 281504707812
  Version= 200.1.9.12 build 55
Devices Overview:
  Number of APs= 1
  Number of Client Devices= 2
  Number of Rogue Devices= 7
Usage Summary:
  Usage of 1 hr:
    Max. Concurrent Users= 2
    Bytes Transmitted= 1.14M
    Number of Rogue Devices= 7
  Usage of 24 hr:
    Max. Concurrent Users= 2
    Bytes Transmitted= 913.39M
    Number of Rogue Devices= 15
Memory Utilization:
  Used Bytes= 77184(kB)
  Used Percentage= 31%
  Free Bytes= 177344(kB)
  Free Percentage= 69%
ruckus#
```

### Ping: 
Just pointing out the obvious....
```
ruckus# ping 198.18.2.118
PING 198.18.2.118 (198.18.2.118): 56 data bytes
64 bytes from 198.18.2.118: seq=0 ttl=64 time=1.243 ms
64 bytes from 198.18.2.118: seq=1 ttl=64 time=0.283 ms
64 bytes from 198.18.2.118: seq=2 ttl=64 time=0.513 ms
--- 198.18.2.118 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.283/0.679/1.243 ms
ruckus# 
```

## From The Linux Shell

### Break out of CLI: 
enter the following command at the CLI to get down to the Linux Shell. 
```
ruckus# !v54! Dj5qmv94ndno3HEfoaVMhsSiG0zIGAOd
Granted to access the Linux Shell.
Exit Ruckus CLI.
Ruckus Wireless ZoneDirector -- Command Line Interface
Enter 'help' for a list of built-in commands.
ruckus$
```

### tcpdump lan interface: 
```
ruckus$ tcpdump -i br0 host 198.18.2.118
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
0 packets captured
0 packets received by filter
0 packets dropped by kernel
ruckus$
```

if you are on the primary AP, you will want to see lots of chatter from the remote units.  If not, you might want to make sure that their discovery agent is working properly.  This is an example of the second AP chatting with the primary. 

```
ruckus$ tcpdump -i br0 host 198.18.2.118
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
17:18:01.673975 IP 198.18.2.118.12225 > 198.18.2.242.12223: LWAPPv0, Data frame, Flags [Last Fragment Bit], length 336
17:18:01.712083 IP 198.18.2.242.12223 > 198.18.2.118.12225: LWAPPv0, Control frame, Flags [Control Bit], length 172
17:18:06.677132 IP 198.18.2.118.12225 > 198.18.2.242.12223: LWAPPv0, Data frame, Flags [Last Fragment Bit], length 336
17:18:06.679455 IP 198.18.2.242.12223 > 198.18.2.118.12225: LWAPPv0, Control frame, Flags [Control Bit], length 172
17:18:11.671320 IP 198.18.2.118.12225 > 198.18.2.242.12223: LWAPPv0, Data frame, Flags [Last Fragment Bit], length 336
```

### remotely log into the other AP: 
Here you are in the primary AP (ap1), and you want to connect to the second AP (ap2) with an address of 198.18.2.118
```
ruckus$ dbclient 198.18.2.118
Please login: super
password :
Copyright(C) 2005-2014 Ruckus Wireless, Inc. All Rights Reserved.
** Ruckus R500 Multimedia Hotzone Wireless AP: 281504507904
rkscli:
```

### Enable Discovery Agent: 

If the secondary unit is not connecting to the master AP, try checking if it's discovery agent is enabled.

```
rkscli: get discovery-agent
Controller Discovery Agent(LWAPP) is disabled.
OK
rkscli:
```

If it's disabled like this one, then you can enable it as such
```
rkscli: set discovery-agent enable
OK
rkscli:
```

### log out of remote APs

to log out of a remote AP, just enter exit and you will undo the dbclient command.  
```
rkscli: exit
Quit:
OK
ruckus$
```

## Check the local Zone Director: 
```
rkscli: get director
------ ZoneDirector Info ------
Device role          : ap
Primary Controller   : 198.18.2.242
Secondary Controller : n/a
DHCP Opt43 Code      : 3
  AP is in Stand-alone mode.
OK
rkscli:
```

The following is a messed up zone director setting
```
rkscli: get director
------ ZoneDirector Info ------
Device role          : ap
Primary Controller   : n/a
Secondary Controller : n/a
DHCP Opt43 Code      : 3
  AP is in Stand-alone mode.
OK
rkscli:
```

### Define Zone Director: 

if you don't have any setting (like above) then you can set who is the primary 
```
rkscli: set director ip 198.18.2.242
** Please reboot for this change to take effect
OK
rkscli:
rkscli: get countrycode
Country is US
OK
rkscli:
rkscli: reboot
OK
rkscli: ruckus$
```

