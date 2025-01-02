# Sniffing Cat Ports

- [Sniffing Cat Ports](#sniffing-cat-ports)
  - [Setting up SPAN on a Cat](#setting-up-span-on-a-cat)
    - [Span a port](#span-a-port)
    - [Span many ports](#span-many-ports)
    - [Span ports on a vlan](#span-ports-on-a-vlan)
    - [Span all ports and trunks on a vlan](#span-all-ports-and-trunks-on-a-vlan)
    - [Enabling/Disabling multiple spans](#enablingdisabling-multiple-spans)
  - [Appendix 1. Using Tcpdump](#appendix-1-using-tcpdump)
  - [Appendix 2. References:](#appendix-2-references)


## Setting up SPAN on a Cat

### Span a port
You can setup a cat switch to send all data traveling though a port to another port (the mirror port). The "mirror" port gets to listen to both sending and receiving data from the original port. It also (by default) can not respond on that port, thus preventing some security issues, as well as any spanning tree issues.

To setup port sniffing on a cat switch, use the set span command as shown:

Spanning a port

```
01   
01  > (enable) set span 3/11 3/9
01  Overwrote Port 3/9 to monitor transmit/receive traffic of Port 3/11 
01  Incoming Packets disabled. Multicast enabled. 
01
01  > (enable) sh span01    
01  Destination     : Port 3/9 
01  Admin Source    : Port 3/11 
01  Oper Source     : Port 3/11 
01  Direction       : transmit/receive 
01  Incoming Packets: disabled 
01  Multicast       : enabled 
01    
01  -------------------------------------------- 
01  > (enable) 	
```


### Span many ports
You can sniff multiple ports from a switch and have the output sent to one monitor port. The syntax for choosing multiple ports is the same for selecting multiple ports for vlans.

Span Multiple Ports

```
01  > (enable) set span 3/11,2/9 3/9
02  Overwrote Port 3/9 to monitor transmit/receive traffic of Port 2/9,3/11 
03  Incoming Packets disabled. Multicast enabled. 
04  > (enable) sh span
05    
06  Destination     : Port 3/9 
07  Admin Source    : Port 2/9,3/11 
08  Oper Source     : Port 2/9,3/11 
09  Direction       : transmit/receive 
10  Incoming Packets: disabled 
11  Multicast       : enabled 
12    
13  -------------------------------------------- 
14  > (enable) 	
```

### Span ports on a vlan
Note that this does not monitor ports that are trunking the vlan, only ports that are set to that vlan only.

### Span all ports and trunks on a vlan
If you want to test sniff multiple ports INCLUDING TRUNK ports, then you can use the commands outlined above for scanning multiple ports, and include the filter option to include only the vlans you are interested in. It is important to note that this command is not available on cat 5500 switches.

Span Ports + Trunks on VLAN

```
01  set span 6/4-5 6/2 filter 2 	
```

- Line 01 takes all traffic on ports 6/4 and 6/5 and filters it to only listen to vlan 2, and then sends that traffic to port 6/2

### Enabling/Disabling multiple spans
To enable a second (or many) separate span on a single switch, you need to add the create flag at the end of the span command. If you want to remove a second span session, use the disable flag as show below.

adding and removing multiple span's

```
01  ! -- view the current single span
02  c4003-n1-1> (enable) sh span
03    
04  Destination     : Port 2/28 
05  Admin Source    : Port 2/25 
06  Oper Source     : Port 2/25 
07  Direction       : transmit/receive 
08  Incoming Packets: disabled 
09  Learning        : enabled 
10  Filter          : - 
11  Status          : active 
12    
13  ------------------------------------------------------------------------ 
14  Total local span sessions:  1 
15  ! 
16  ! -- create a second span
17  c4003-n1-1> (enable) set span 2/30 3/3 create
18  Overwrote Port 3/3 to monitor transmit/receive traffic of Port 2/30 
19  Incoming Packets disabled. Learning enabled. 
20  c4003-n1-1> (enable)  
21  2003 Oct 29 11:04:27 PST -08:00 %SYS-5-SPAN_CFGSTATECHG:local span session active for estination port 3/3 
22  c4003-n1-1> (enable)  
23  ! 
24  ! -- view both spans  
25  c4003-n1-1> (enable) sh span
26    
27  Destination     : Port 2/28 
28  Admin Source    : Port 2/25 
29  Oper Source     : Port 2/25 
30  Direction       : transmit/receive 
31  Incoming Packets: disabled 
32  Learning        : enabled 
33  Filter          : - 
34  Status          : active 
35    
36  ------------------------------------------------------------------------ 
37  Destination     : Port 3/3 
38  Admin Source    : Port 2/30 
39  Oper Source     : Port 2/30 
40  Direction       : transmit/receive 
41  Incoming Packets: disabled 
42  Learning        : enabled
43  Filter          : - 
44  Status          : active 
45    
46  ------------------------------------------------------------------------ 
47  Total local span sessions:  2 
48  c4003-n1-1> (enable) 
49  ! 
50  ! -- remove one span
51  c4003-n1-1> (enable) set span disable 3/3
52  This command will disable your span session. 
53  Do you want to continue (y/n) [n]?y
54  Disabled Port 3/3 to monitor transmit/receive traffic of Port 2/30 
55  Incoming Packets disabled. Learning enabled. 
56  c4003-n1-1> (enable)  
57  2003 Oct 29 11:08:04 PST -08:00 %PAGP-5-PORTFROMSTP:Port 3/3 left bridge port 3/3 
58  2003 Oct 29 11:08:04 PST -08:00 %SYS-5-SPAN_CFGSTATECHG:local span session inactive for destination port 3/3 
59  2003 Oct 29 11:08:18 PST -08:00 %PAGP-5-PORTTOSTP:Port 3/3 joined bridge port 3/3 
60  c4003-n1-1> (enable)  
61  ! 
62  ! -- show that there is still one span left
63  c4003-n1-1> (enable) sh span
64    
65  Destination     : Port 2/28 
66  Admin Source    : Port 2/25 
67  Oper Source     : Port 2/25 
68  Direction       : transmit/receive 
69  Incoming Packets: disabled 
70  Learning        : enabled 
71  Filter          : - 
72  Status          : active 
73    
74  ------------------------------------------------------------------------ 
75  Total local span sessions:  1 
76  c4003-n1-1> (enable)  	
```

## Appendix 1. Using Tcpdump
You need to have supervisor rights to run tcpdump, then you will need to run tcpdump specifying which port to sniff. The following will sniff traffic from interface hme1 and cat it to the display.

```
> sudo -s 	# /usr/local/sbin/tcpdump -i hme1
```


That is ok for the most basic needs but there are many other flags you can use to get just the right information out of the sniffer. To find out on the details, check the man page. I have a couple of special ones that I will point out below, because I use them allot. I should point out that I like to save the data to disk and then view it later, rather then have it quickly scroll of the screen. The -w flag specifies what file to save the dump file as. Also tcpdump will by default only grab a part of each packet to save space. The -s flag specifies how many bytes for each packet to save. Since ethernet packets can only be a max of 1500 bytes (not including tagging), saving 2000 bytes should be enough.


to get the host to dump all data (don't drop any data):
```
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump
```

to do the same, but only dump data to or from a specific host:
```
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump host 10.11.128.20
```

to do the same, but only collect data between two hosts:
```
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump host 10.11.128.20 and 172.22.1.129
```

to do the same, but between two hosts, and a specific protocol: (note that "and" can be replaced with "or")
```
/usr/local/sbin/tcpdump -i hme1 -s 2000 -w dump host 10.11.128.20 and 172.22.1.129 and icmp
```


to send to file, and compress the file:
```
tcpdump -l -i fxp1 -w -  | gzip > /tmp/bar.gz
```


note that here, gzip needs a certain amount of data buffered before it can zip up anything. so the "zipping" is done in quantum steps. you might loose some of the ending data if the input is slow, and you stop the dump right after an important action.

Since I like to save the dump file and view it off line, I have the ability to view it with tools other then tcpdump. I prefer to use Ethereal. It installs on both unix and windows, so if your stuck on a win box, your still in luck. (so to speak)

## Appendix 2. References:
- [Configuring the Catalyst Switched Port Analyzer (SPAN)](http://www.cisco.com/warp/customer/473/41.html)
- [Configuring SPAN and RSPAN on Cat 4000's](http://www.cisco.com/univercd/cc/td/doc/product/lan/cat4000/7_5/config/span.htm)
- [Documentation: Catalyst 4000 Family Switches](http://www.cisco.com/univercd/cc/td/doc/product/lan/cat4000/)
- [Catalyst 6.3-6.4 Command Reference](http://www.cisco.com/univercd/cc/td/doc/product/lan/cat4000/rel6_3/cmd_ref/cr_toc.htm)