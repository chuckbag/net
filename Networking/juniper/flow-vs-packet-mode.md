# Flow vs. Packet Mode

## Overview: 
By default, Junos boxes come in a "firewall mode" where the systems default-block traffic, and all ACL's are stateful.  If you are only pushing bits and would rather the device run in a stateless mode, this can be done with a quick command and a reboot.  (You can also run in a mode where some interfaces are stateless and some are stateful.)

Juniper calls its stateful setting as flow-based forwarding, and it stateless mode as packet-based forwarding. 

An overview of the benefits of either are outlined as follows:

### Packet Based Forwarding

Traffic is processed by the packet-based forwarding module providing all of the following services: 
- Routing 
- Quality of service (QoS)
- Link fragmentation and interleaving (LFI)
- Generic routing encapsulation (GrE) and IP over IP (IP-IP) tunneling (no fragmentation and reassembly)
- L2 switching
- MPLS
- IPv6
- Compressed real-Time Transport Protocol (CrTP)

To enable packet based forwarding (stateless), commit the following and reboot the box.  
```bash
> set security forwarding-options family mpls mode packet-based
> request system reboot
```

### Flow Based Forwarding

The flow-based forwarding module was able to provide all of the above and also deliver the following services:
- Stateful inspection firewall (including screens)
- NAT
- IPsec
- J-Flow
- Intrusion detection and prevention
- UTM
- GrE fragmentation and reassembly

To enable flow based forwarding (statefull - the default), commit the following and reboot the box.  
```bash
> set security forwarding-options family mpls mode flow-based
> request system reboot
```



## Stateless ACLs: 
Once you are in packet-based mode, you can throw everything under the security configs away.  In fact, to commit the packet-mode, you probably needed to issue the command:
```
delete security
```

The router at this point is a wonderfully simple packet pusher that default-allows all traffic.  

If you want to setup ACL's, you use the (wonderfully confusing) firewall family commands.  

Here's a simple example to control access to the loopback interface, such that it's bgp peer can talk to it, and the admins can ssh to it. 
```
! allow admins to ssh to the loopback: 
set firewall family inet filter protect-RE term ssh-term from source-address 192.168.122.0/24 
set firewall family inet filter protect-RE term ssh-term from protocol tcp 
set firewall family inet filter protect-RE term ssh-term from destination-port ssh
set firewall family inet filter protect-RE term ssh-term then accept
! allow bgp peer to connect to loopback: 
set firewall family inet filter protect-RE term bgp-term from source-address 10.2.1.0/24 
set firewall family inet filter protect-RE term bgp-term from protocol tcp 
set firewall family inet filter protect-RE term bgp-term from destination-port bgp 
set firewall family inet filter protect-RE term bgp-term then accept 
! block and log everything else: 
set firewall family inet filter protect-RE term discard-rest-term then log 
set firewall family inet filter protect-RE term discard-rest-term then syslog 
set firewall family inet filter protect-RE term discard-rest-term then discard
! bind the filter "protect-RE" to the loopback interface: 
set interfaces lo0 unit 0 family inet filter input protect-RE
```

Here's another funky example allowing all transit traffic though except udp and tcp, but allowing ssh and telnet though.  
```
! allow all traffic except tcp + udp:
set firewall family inet filter filter1 term term1 from protocol-except tcp
set firewall family inet filter filter1 term term1 from protocol-except udp
set firewall family inet filter filter1 term term1 then accept
! block traffic from private ip range: 
set firewall family inet filter filter1 term term2 from address 192.168.0.0/16
set firewall family inet filter filter1 term term2 then reject
! allow telnet and ssh traffic though: 
set firewall family inet filter filter1 term term3 from destination-port ssh
set firewall family inet filter filter1 term term3 from destination-port telnet
set firewall family inet filter filter1 term term3 then accept
! block all other traffic:
set firewall family inet filter filter1 term term4 then reject
! create wan interface, and bind filter "filter1" to it: 
set interfaces ge-0/0/1 unit 0 family inet address 10.1.2.3/30
set interfaces ge-0/0/1 unit 0 family inet filter input filter1
```


## References: 
- [Branch SRX Series and J Series Selective Packet Services](http://www.juniper.net/us/en/local/pdf/app-notes/3500192-en.pdf): Juniper Networks, 2010, >v.9.2
- [Firewall Filters Feature Guide for Routing Devices](http://www.juniper.net/techpubs//en_US/junos/information-products/pathway-pages/config-guide-firewall-filter/config-guide-firewall-filter.html#configuration): 
- [How to enable the IPv6 flow (or packet) mode on SRX](http://kb.juniper.net/InfoCenter/index?page=content&id=KB25697&smlogin=true): Juniper KB25697