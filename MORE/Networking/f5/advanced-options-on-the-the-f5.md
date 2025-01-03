# Advanced Options on the the F5

## Accessing the F5
- Via your browser, you can do everything.  You don't have shell access, but any bigpipe command can be entered in the bigpipe window (this includes -s that shows the running config).
- Via a ssh telnet application.  Use port 22 ssh telnet and at the prompt, enter your username and password.

## Changing the Configuration
All configs are stored in the Big/IP, in the /etc/ directory.  When a new config is made, a brief description should be added to the beginning of the config, commented out with hashes (#).

Once this is done, you can check to see if there are any syntax problems by running the bigpipe command:

```
bigpipe -f -d /etc/big.v{version#).conf
```

To actually enable that config, at the bigpipe command type:<br>
```
bigpipe -f /etc/big.v{version#).conf
```
 

(note that if you are doing this from the browser, you do not need to type the word "bigpipe", and you would have needed to upload the config earlier.)

## Watching the connections
To see an update of the traffic to each service and vip, and what services are up or down use the bigtop command.  At the shell prompt, type:
```
bigtop
```

## Snooping the interfaces
Remember that the firewall hides the clients ip address!!  

### TCPDUMP QUICK PRIMER (sch 990715)

Also reference the "man tcpdump" on BIG/ip

Example Syntax: You probably don't need all of these at once.
```
tcpdump -i exp0 -vv -e -n -X -s 256 host 192.168.1.100 and port 80 >> /var/tmp/exp0-dump.txt

-i     = exp0 is the external(generally)and exp1 is the internal interface; 
         other possible interfaces are de0, fpa0, &amp; hmc0
-vv    = (that's v v) very verbose mode - supplies additional header info
-e     = displays mac addresses (link level)as well as ip addresses
-X     = turns on ascii and hex dumping of packet data
-s 256 = the packet payload window size (default is 76 and bigger is slower; use 1518 for 
         ethernet packet size)
-n     = displays ip instead of names (no name lookup)
host <ip>= provides packets to/from that host
net    = allows specifying a network (192.168.101) 
and or = boolean operators
port &lt;port&gt; = filters for port name or number. Can be udp or tcp or icmp or http....etc.
tcp    = any tcp packets
udp    = any udp packets
icmp   = guess
```

"`>> /var/tmp/exp0-dump.txt`"  will append the data to an ascii file in `/var/tmp` (which is the BIG/ip directory I recommend you use for saving dumps). Tcpdump also has a `-w <fn>` parm that will save a binary and can be read back in with tcpdump after the dump.

### Other tcpdump commands syntax examples:
```
BigIP1:~# tcpdump -i exp0 -X -s 192 net 10.0.0 <enter>
BIGIP1:~# tcpdump -envv -i exp0 host 207.17.117.200 and port 80 <enter>
BIGIP1:~# tcpdump -n -vv -i exp0 host 207.17.117.200 and icmp >> exp0-dump.txt <enter>
bigip1:~# tcpdump -i exp1 -envv host <ip> and host <ip> and tcp <enter>
```

### Other useful BIG/ip commands
```
trafshow <enter>   # will dynamically display network connections; most active on top
trafshow -i exp1 <enter>   # show the exp1 interface
trafshow -h        # help
```

The "`bigpipe dt`" command can also display the connection that bigip has made. Output is one connection per line. 

It is very dynamic and verbose if the site is busy. It is the current connection table and should be run quickly after an http test client hits the VIP because http connections are brief.

I would try:
```
bigpipe dt <enter>  # dumps the entire table to the screen
bigpipe dt | wc -l <enter> # sends the putput to wc -l to count the connections (lines)
bigpipe dt | grep <ip>:80 |wc -l <enter>  # this will count the current http connections for <ip> 
bigpipe dt | grep <ip_of_client> <enter>
bigpipe dt | grep <ip_of_client> >> /var/tmp/tabledump.txt <enter>
```

Also Note the configuration when sniffing:

Below is a traffic flow between a client at ip 192.168.235.105 and the server at 192.168.235.238.  A special note is that of the VIP's at the Big/ip.  They handle the load balancing between the two services in the pool (ie: mem1 & mem2 use VIP 192.168.230.20).

```
     /-----------\                                 .     
     |  client   |                                 .     
     \-----+-----/  <- 192.168.235.105             .     
           |                     (or whatever)     .     
           v                                       .     
     /-----+-----\  <- 192.168.235.1               .     
     | firewall  |            (216.34.101.173)     .   within the big/ip:              
     \-----+-----/  <- 192.168.230.1               .        (exp0)  | https             
           ^                                       .   /------------+------------\ 
           |                                       .   |  https://mem.smcint.com |      
     /-----+-----\  <- 192.168.230.221             .   |            |            | 
     |  big/IP   |                                 .   |            V            | 
     \-----+-----/  <- 192.168.235.221             .   |     192.168.230.20      | 
           ^                                       .   \------------+------------/ 
           |                                       .         (exp1) | http
     /-----+-----\  <- 192.168.235.238             .                V
     | NS Server |               (or whatever)     .         192.168.235.238
     \-----------/        
```

## Few Notes on Setting up a System

### First-Time Boot Utility
When the BIP-IP is set with it's factory defaults, it automatically boots to it's first-time boot utility.  This asks for the basic info for the F5 to be setup.  This includes the following things:

- Root Password
- Host Name
- Default Route
- Time Zone
- DNS Forwarding Proxy
- Interface settings for each network interface
- Configuration for the BIG-IP Controller redundant systems (fail-over IP address)
- IP address for remote administration
- Settings for the web server on the Big-IP Controller

To re-run this utility, at the command prompt, type config

### Testing and Modifying Interface Attributes
After the First-Time Boot Utility, we need to test to make sure that the interfaces were configured correctly.  We might have gotten mixed up and configured e0 as e1 (or something).

Check the configurations

To view the configuration of the interfaces,
```
ifconfig -a
```

If you can't ping the next hop, try sniffing the connection.  

Change the Interface Configuration

If you realize that the interfaces were setup wrong, either the wrong IPs or the wrong interfaces, you can change them by altering the following files:
- `/etc/hosts`:  This defines what ips go to what interfaces, the default route, the hosts name, and if their considered internal or external.
- `/etc/netstart`: This defines the interfaces, what their ip/mask/interface/broadcast/media is.
- `/etc/bigip.conf`: This is the main configuration file for the BIG-IP.  It includes what interfaces are considered internal and external.

When your done modifying these files, it's best to just reboot the host, and make sure that it comes up clean.

