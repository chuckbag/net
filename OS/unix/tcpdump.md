# tcpdump

- [tcpdump](#tcpdump)
  - [Overview:](#overview)
  - [Examples:](#examples)
    - [Dumping to a file:](#dumping-to-a-file)
    - [Cating to the Console:](#cating-to-the-console)
  - [Grabbing multiple hosts and ports:](#grabbing-multiple-hosts-and-ports)
    - [Examples:](#examples-1)
    - [Log Rotation:](#log-rotation)
  - [Tracking Interfaces:](#tracking-interfaces)
  - [References:](#references)

## Overview:

You need to have supervisor rights to run tcpdump, then you will need to run tcpdump specifying which port to sniff. The following will sniff traffic from interface hme1 and cat it to the display.
```bash
> sudo -s
# /usr/local/sbin/tcpdump -i hme1
```
That is ok for the most basic needs but there are many other flags you can use to get just the right information out of the sniffer. To find out on the details, check the man page. I have a couple of special ones that I will point out below, because I use them allot. I should point out that I like to save the data to disk and then view it later, rather then have it quickly scroll of the screen. The -w flag specifies what file to save the dump file as. Also tcpdump will by default only grab a part of each packet to save space. The -s flag specifies how many bytes for each packet to save. Since ethernet packets can only be a max of 1500 bytes (not including tagging), saving 2000 bytes should be enough.

## Examples:
### Dumping to a file: 

To get the host to dump all data (don't drop any data):
```bash
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump
```
Where
- `-i hme1` captures data from the Interface "hme1"
- `-s 2000` captures the first 2000 bytes of each packet
- `-w dump` puts all the captured files into a pcap file called dump

to do the same, but only dump data to or from a specific host:
```bash
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump host 10.11.128.20
```
Where
- `-i hme1` captures data from the Interface "hme1"
- `-s 2000` captures the first 2000 bytes of each packet
- `-w dump` puts all the captured files into a pcap file called dump
- `host 10.11.128.20` filters out all traffic except ones coming from or going to the IP address 10.11.128.20

to do the same, but only collect data between two hosts:
```bash
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump host 10.11.128.20 and 172.22.1.129
```

Where
- `-i hme1` captures data from the Interface "hme1"
- `-s 2000` captures the first 2000 bytes of each packet
- `-w dump` puts all the captured files into a pcap file called dump
- `host 10.11.128.20 and 172.22.1.129` filters out all traffic except ones coming from or going to the two IP address 10.11.128.20 and 172.22.1.129

to do the same, but between two hosts, and a specific protocol: (note that "and" can be replaced with "or")
```bash
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump host 10.11.128.20 and 172.22.1.129 and icmp
```

Where
- `-i hme1` captures data from the Interface "hme1"
- `-s 2000` captures the first 2000 bytes of each packet
- `-w dump` puts all the captured files into a pcap file called dump
- `host 10.11.128.20 and 172.22.1.129 and icmp` filters out all traffic except icmp coming from or going to the two IP address 10.11.128.20 and 172.22.1.129

to send to file, and compress the file:
```bash
tcpdump -l -i fxp1 -w -  | gzip > /tmp/bar.gz
```

Where
- `-l` buffer the stdout buffered so that you can see the traffic and capture it
- `-i fxp1` captures data from the interface "fxp1"
- `-w - | gzip > /tmp/bar.gz` takes the output, and instead of writing it directly to a file, pipes it do gzip which compresses it, and then saves it to /tmp/bar.gz file

note that here, gzip needs a certain amount of data buffered before it can zip up anything. so the "zipping" is done in quantum steps. you might loose some of the ending data if the input is slow, and you stop the dump right after an important action.

### Cating to the Console: 
view interfaces that you can sniff: 
```bash
[root@web01 ~]# tcpdump -D
1.eth0
2.bond0
3.eth1
4.any (Pseudo-device that captures on all interfaces)
5.lo
```

only capture the first 10 packets (c 10), show it in human readable format (A), and only show traffic over port 443 (port 443)
```bash
tcpdump -Ac 10 port 443
```

buffer output (`l`), don't resolve dns names (`N`), stop after 50 packets (`c 50`), don't collect traffic from host 10.120.81.17 (`not host 10.120.81.17`) and (`and`) don't collect ssh traffic (`not port 22`).  Also only collect from the bond0 interface (`-i bond0`)
```bash
tcpdump -lNc 50  not host 10.120.81.17 and not port 22 -i bond0
```

## Grabbing multiple hosts and ports: 
You can capture multiple hosts and you can write it over multiple lines for clarity with the following: 
```bash
/usr/sbin/tcpdump -i eth2 \
host 10.120.128.252 or host 10.120.128.253 or \
host 10.55.128.252 or host 10.55.128.253 or \
host 212.111.45.154 or \
host 199.233.203.69 \
-s 0 -w border_net.pcap
```


### Examples:

Only show numbers in flows.  (dont' decode hostnames or port names)
- `-p` : not in promiscuous mode (if your not on a hub/span port, its the same thing)
- `-nn` : don't decode hostnames or port names
- `-q` : "quick" Just show the flows with very little other data.

```bash
sudo /usr/sbin/tcpdump -p -nnq

16:58:19.989187 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989268 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989349 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989429 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989510 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989591 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989672 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
16:58:19.989763 IP 10.50.176.32.22 > 10.50.34.129.11922: tcp 48
```

### Log Rotation: 
You run a TCP dump running continuously, and have it create files of a specific size.  You can also define how many files can be stored on the server at a time before being written over.  

Save files of 100MB: 
```bash
tcpdump -s 0 -C 100 -w dump.pcap
```
Note that this will continuously create 100MB files until the machines disks fill up.  

Save only 10 100MB files, and FIFO out the rest. 
```bash
tcpdump -s 0 -C 100 -W 10 -w dump.pcap
```
You will see ten files `dump.pcap1 .... dump.pcap10`.  Once "10" is written to, tcpdump will clear `dump.pcap1` and write to it.  

Running the dump in the background, and having all output logged to the file "nohup.out".  
```bash
nohup tcpdump -s 0 -C 100 -W 10 -w dump.pcap &
```

## Tracking Interfaces: 
What switch port is your sniffer plugged into?  If your remote to the sniffer, this can sometimes be tricky to figure out.  One easy method is to shut the interface on the switch, and then check if the link went down on the server.  To check link status on the server, cat the contents of the carrier file.  If the value is 1, then the interface has link, if it's 0 then the interface has no link.  

```bash
[sniff01 ~]$ sudo cat /sys/class/net/eth2/carrier
0
[sniff01 ~]$ sudo cat /sys/class/net/eth2/carrier
1
```

## References: 
- [Sniffing (span) on IOS](): 
- [How to detect the physical connected state of a network cable/connector](http://stackoverflow.com/questions/808560/how-to-detect-the-physical-connected-state-of-a-network-cable-connector)?
- [tcpdump with rotating capture-files](https://clutterbox.de/2010/08/tcpdump-with-rotating-capture-files/), Alexander, Aug 2010
- [HOW TO IMPLEMENT MAX FILE SIZE LIMITS AND “LOG ROTATION” WITH TCPDUMP](http://mikeberggren.com/post/55090292716/tcpdump-max), Mike Berggren, 2015
- [tcpdump man page](http://www.tcpdump.org/tcpdump_man.html): list of all the instructions for tcpdump