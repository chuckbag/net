# iptables

- [iptables](#iptables)
  - [Viewing Status](#viewing-status)
  - [Edit the rules:](#edit-the-rules)
  - [Log dropped packets](#log-dropped-packets)
    - [Note on the Log command:](#note-on-the-log-command)
  - [References:](#references)

## Viewing Status
Is it running?
```bash
# service iptables status
Firewall is stopped.
```

What is allowed: 
```bash
$ sudo /sbin/iptables --list
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
RH-Firewall-1-INPUT  all  --  anywhere             anywhere

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
RH-Firewall-1-INPUT  all  --  anywhere             anywhere

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

Chain RH-Firewall-1-INPUT (2 references)
target     prot opt source               destination
ACCEPT     tcp  --  anywhere             anywhere            tcp dpt:amqp
ACCEPT     tcp  --  anywhere             anywhere            state NEW tcp dpt:webcache
ACCEPT     tcp  --  1.1.1.1              anywhere            state NEW tcp dpt:epmd
ACCEPT     udp  --  1.1.1.1              anywhere            state NEW udp dpt:epmd
ACCEPT     udp  --  1.1.1.1              anywhere            state NEW udp dpts:newoak:pxc-roid
ACCEPT     tcp  --  1.1.1.1              anywhere            state NEW tcp dpts:newoak:pxc-roid
ACCEPT     tcp  --  anywhere             anywhere            state NEW tcp dpt:55672
ACCEPT     all  --  anywhere             anywhere
ACCEPT     icmp --  anywhere             anywhere            icmp any
ACCEPT     all  --  anywhere             anywhere            state RELATED,ESTABLISHED
ACCEPT     tcp  --  anywhere             anywhere            state NEW tcp dpt:ssh
REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited
$
```

## Edit the rules: 
Shut down iptables before making a change: 
```
systemctl stop iptables
```

modify the iptables ACL list
```
vim /etc/sysconfig/iptables
```

add a string allowing some field.  In this example, the red allows ssh inbound from a specific ClassC network. 
```
# Generated by iptables-save v1.4.21 on Tue Dec 26 14:52:29 2017
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [2098:33328884]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT

-A INPUT -s 10.33.32.0/24 -p tcp -m state --state NEW -m tcp --dport 22 -m comment --comment "Allow in-bound SSH from a friendly network" -j ACCEPT

-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
# Completed on Tue Dec 26 14:52:29 2017
```

Save the changes and re-enable iptables
```bash
[root@box1 ~]# service iptables save
iptables: Nothing to save.                                 [WARNING]
[root@box1 ~]# vim /etc/sysconfig/iptables
[root@box1 ~]# systemctl start iptables
[root@box1 ~]# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
ACCEPT     tcp  --  10.33.32.0/24        0.0.0.0/0            state NEW tcp dpt:22 /* Allow in-bound SSH from the Juniper VPN subnet */
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
[root@box1 ~]# systemctl enable iptables
[root@box1 ~]# 
```

## Log dropped packets

(remove the `REJECT` lines at the end of the current table) and create a new chain called LOGGING at the end of your iptables list.  In the new chain, log and drop everything from there: 

Drop all outbound and log: 
```
-N LOGGING
-A INPUT -j LOGGING
-A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 --log-ip-options
-A LOGGING -j DROP
```

- `-m limit`: This uses the limit matching module. Using this you can limit the logging using –limit option.
- `--limit 2/min`: This indicates the maximum average matching rate for logging. In this example, for the similar packets it will limit logging to 2 per minute. You can also specify 2/second, 2/minute, 2/hour, 2/day. This is helpful when you don’t want to clutter your log messages with repeated messages of the same dropped packets.
- `-j LOG`: This indicates that the target for this packet is LOG. i.e write to the log file.
- `--log-prefix “IPTables-Dropped`: ” You can specify any log prefix, which will be appended to the log messages that will be written to the /var/log/messages file
- `–log-level 4` This is the standard syslog levels. 4 is warning. You can use number from the range 0 through 7. 0 is emergency and 7 is debug.
- `--log-ip-options`  Log IP and above.  Don't fill up your logs with blocked ethernet logs


### Note on the Log command: 
```
  LOG
       Turn on kernel logging of matching packets.  When this option 
       is set for a rule, the Linux kernel will print some 
       information  on  all  matching  packets
       (like most IP header fields) via the kernel log (where it can 
       be read with dmesg or syslogd(8)).  This is a "non-terminating 
       target", i.e. rule traversal
       continues at the next rule.  So if you want to LOG the packets 
       you refuse, use two separate rules with the same matching 
       criteria, first using target LOG
       then DROP (or REJECT).

       --log-level level
              Level of logging (numeric or see syslog.conf(5)).

       --log-prefix prefix
              Prefix log messages with the specified prefix; up to 29 
              letters long, and useful for distinguishing messages in 
              the logs.

       --log-tcp-sequence
              Log TCP sequence numbers. This is a security risk if the 
              log is readable by users.

       --log-tcp-options
              Log options from the TCP packet header.

       --log-ip-options
              Log options from the IP packet header.

       --log-uid
              Log the userid of the process which generated the packet.
```



## References: 
- [Centos IPTables](http://wiki.centos.org/HowTos/Network/IPTables)
- [How to Log Linux IPTables Firewall Dropped Packets to a Log File](https://www.thegeekstuff.com/2012/08/iptables-log-packets/): The Geek Stuff, Ramesh Natarajan, 8/12
- [Linux Iptables Firewall](https://www.cyberciti.biz/tips/iptables-log-network-layer-ip-tcp-headers.html): Log IP or TCP Packet Header: nixCraft, Vivek Gite, 1/08

