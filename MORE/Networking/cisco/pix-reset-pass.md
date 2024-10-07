# pix-reset-pass

Setup tftpd

Download the os you want on the pix

Immediately after supplying power to the pix, send a BREAK signal on the console port.

At the Pix monitor prompt:
```
monitor> interface 0
0: i8255X @ PCI(bus:0 dev:17 irq:9 )
1: i8255X @ PCI(bus:0 dev:18 irq:10)
Ethernet auto negotiation timed out.
Ethernet port 0 could not be initialized.
monitor> address 192.168.0.123
address 192.168.0.123
monitor> server 192.168.0.30
server 192.168.0.30
monitor> file np70.bin
file np70.bin
monitor> gateway 192.168.0.30
gateway 192.168.0.30
monitor> ping 192.168.0.30
Sending 5, 100-byte 0xe29e ICMP Echoes to 192.168.0.30, timeout is 4 seconds:
Success rate is 0 percent (0/5)
monitor> tftp
tftp np63.bin@192.168.0.30 via 192.168.0.30.....................................
................................................................................
................................................................
Received 92160 bytes
Cisco Secure PIX Firewall password tool (3.0) #0: Thu Jul 17 08:01:09 PDT 2003
Flash=E28F640J3 @ 0x3000000
BIOS Flash=E28F640J3 @ 0xD8000
Do you wish to erase the passwords? [yn] y
The following lines will be removed from the configuration:
        enable password uh26dnBSj158NxuE encrypted
        passwd zvomGh1hiLRwA5mM encrypted
Do you want to remove the commands listed above from the configuration? [yn] y
```


## Reference:
- [Password Recovery and AAA Configuration Recovery Procedure for the PIX](http://www.cisco.com/en/US/products/hw/vpndevc/ps2030/products_password_recovery09186a008009478b.shtml)