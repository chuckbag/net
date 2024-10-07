# Upgrade the OS

The firewall probably did not ship with the correct os.  Grab a TFTP server (win/linux) and upgrade the firewall.

```bash
ciscoasa(config)# copy tftp://192.168.1.10/asa842-k8.bin flash:
Address or name of remote host [192.168.1.10]?
Source filename [asa842-k8.bin]?
Destination filename [asa842-k8.bin]?
Accessing tftp://192.168.1.10/asa842-k8.bin...!!!!!!!!!!!!!!!!!!
[...]
ciscoasa(config)# boot system flash:/asa842-k8.bin
INFO: Converting flash:/asa842-k8.bin to disk0:/asa842-k8.bin
ciscoasa(config)#
ciscoasa(config)# wr mem
Building configuration...
Cryptochecksum: 1ee2400f 42bb77aa 734c4a85 f7ee409e
2435 bytes copied in 3.340 secs (811 bytes/sec)
[OK]
ciscoasa(config)# reload
Proceed with reload? [confirm]
ciscoasa(config)#
***
*** --- START GRACEFUL SHUTDOWN ---
Shutting down isakmp
```

## Installing the OS on a blank firewall. 

One would think that you could use the USB socket on the front of the firewall to upload the OS on, right?!?! [Nope!  the USB socket is just for show.  Cisco has not figured out how to use it just yet](). 
To upload an OS on an ASA with no system image, first plug your PC directly into the g0/0 interface, and set your IP to `1.1.1.1`. (You might also want to [confirm you have enough memory](http://www.cisco.com/c/en/us/td/docs/security/asa/compatibility/asamatrx.html).)  Also setup a tftp server on your PC and make the OS image for the firewall available.  (good apps for this would be MobiaXterm, or tftpd32).

Then from the console of the firewall at boot time, send the BREAK signal to get to the rommon prompt. Then enter in the following commands: 

```
ADDRESS=1.1.1.2
GATEWAY=1.1.1.1
SERVER=1.1.1.1
PORT=GigabitEthernet0/0
IMAGE=asa845-k8.bin
set
ping 1.1.1.1
tftp
```

This will get your firewall to boot directly from the tftp server.  This is a good start, but the OS is not yet on disk yet.  You need to do the following too!
First look will show you that you don't have any interfaces set

```shell
ciscoasa# sh int ip br
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         unassigned      YES unset  administratively down up
GigabitEthernet0/1         unassigned      YES unset  administratively down up
GigabitEthernet0/2         unassigned      YES unset  administratively down down
GigabitEthernet0/3         unassigned      YES unset  administratively down down
Management0/0              unassigned      YES unset  administratively down down
ciscoasa# 
```

So set up an interface

```bash
ciscoasa# conf t
ciscoasa(config)#
ciscoasa(config-if)# ip add 1.1.1.2 255.255.255.0
ciscoasa(config-if)# security-level 0
ciscoasa(config-if)# nameif out
ciscoasa(config-if)# no shut
ciscoasa(config-if)# end
```

And then confirm that it works.

```bash
ciscoasa# sh int ip brief
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         1.1.1.2         YES manual up                    up
GigabitEthernet0/1         unassigned      YES unset  administratively down up
GigabitEthernet0/2         unassigned      YES unset  administratively down down
GigabitEthernet0/3         unassigned      YES unset  administratively down down
Management0/0              unassigned      YES unset  administratively down down
ciscoasa#
ciscoasa# ping 1.1.1.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
ciscoasa# ping 1.1.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
ciscoasa#
```

## References: 
- [Boot Cisco ASA From TFTP (Upgrade from ROMMON)](http://www.petenetlive.com/KB/Article/0000792.htm):