# ntpd


- [ntpd](#ntpd)
  - [Overview:](#overview)
  - [ntpdate](#ntpdate)
  - [ntpd - the ntp daemon](#ntpd---the-ntp-daemon)
    - [Where do I get correct time from?](#where-do-i-get-correct-time-from)
      - [Accurate time:](#accurate-time)
      - [Good Enough Time:](#good-enough-time)
    - [Setting up ntpd as a server](#setting-up-ntpd-as-a-server)
    - [Setting up ntpd as a client](#setting-up-ntpd-as-a-client)
      - [Multicast Time:](#multicast-time)
      - [Unicast Time:](#unicast-time)
    - [Starting (or stopping) nptd (centos 6)](#starting-or-stopping-nptd-centos-6)
    - [Starting (or stoping) nptd (centos 7)](#starting-or-stoping-nptd-centos-7)
  - [ntpq - Diagnostic Tool](#ntpq---diagnostic-tool)
    - [Checking to see if ntpd is running](#checking-to-see-if-ntpd-is-running)
    - [Checking external clocks:](#checking-external-clocks)
    - [Checking to see if ntp is multicasting](#checking-to-see-if-ntp-is-multicasting)
  - [Sniffing:](#sniffing)
    - [Unicast:](#unicast)
    - [Multicast:](#multicast)
  - [References:](#references)

## Overview:
Setting up a time service either so the current host gets correct time, or to distribute time to other systems. 

## ntpdate
This app is similar to the unix 'date' command in that it lets you set the clock of your system. But where the date command has you enter the correct time, the ntpdate command has you enter a ntp server (who has the correct time). For example, to set your hosts clock correctly, you would enter the command:

```bash
# ntpdate tick.mit.edu
13 Apr 18:32:36 ntpdate[25523]: step time server 18.145.0.30 offset 1.542282 sec
#
```

You can also use it to set the time of your server, and this can be helpful before stating up NTP if your servers clock is off by a lot of time. 

```bash
# ntpdate 0.pool.ntp.org
18 Apr 14:05:52 ntpdate[2531]: adjust time server 66.228.39.48 offset 0.183101 sec
#
```

## ntpd - the ntp daemon
This is a daemon that will run continuously and keep your hosts time always accurate. This is setup so it's pretty easy to configure and run. There are two main ways that I will setup xntpd to run, one as a server, that broadcasts out the time for everyone else to hear, and the other as a client, who listens for the broadcasts, or who queries stratum 2 clocks for the time.
All configurations to make xntpd do one thing or the other, is done via the ntpd.conf file. The distribution comes with two pre-packaged config files; ntp.client, and ntp.server. All you need to do is cut and paste what you want, and create the file `/etc/ntp.conf`.

### Where do I get correct time from?
Ideal, you would have your own stratum 0 time source, a clock that sets itself from an atomic clock (ie GPS). But normally, you'll get your time from some other server (a stratum 2+) on the internet.

#### Accurate time:
For a server that needs exact time, you will want to point directly to three or more specific clocks.  You can find a [list of public clocks at the ntp site](http://support.ntp.org/bin/view/Servers/WebHome#Browsing_the_Lists).  Make sure though, that you chose **at least three servers**, as this makes xntpd the happiest. 

#### Good Enough Time:
If you have a end host or desktop that just needs to know the correct time, but does not to be accurate to the microsecond, you can simply point to [one of the ntp pools out there](http://www.pool.ntp.org/en/).  The pools will balance your queries to hundreds or thousands of servers in your geographic region.  Because of this, you can not get very accurate time, but if you are only pointing to one time server anyway, this is more then good enough. 


### Setting up ntpd as a server
You will want to create the `ntp.conf` file such that it gets its time from at least 3 servers, and that it multicast's its time out. This is an example of my file, the server is based in California, USA. So the servers that it get's its time from are located closer to it.
```txt
server ntp1.sf-bay.org
server ntp.ucsd.edu
server tick.mit.edu

logfile /var/log/ntp.log

broadcast 224.0.1.1 ttl 4  #announce time via multicast
```

### Setting up ntpd as a client
Here there are two different ways to configure your host. One is if there is a multicast server sending out time that your client can receive.  The other is a stand alone version, where it asks the time directly from another time source .

#### Multicast Time:
If there is a multicast time broadcast that you can listen to, modify the ntp.conf file by replacing it with the .client example that comes with the distribution. 
```bash
copy /etc/ntp.client to /etc/ntp.conf 
```

#### Unicast Time:
It there is no multicast stream, and you need to configure your host to talk directly to time servers, the easiest solution is to point your server directly to the [time pools](http://www.pool.ntp.org/en/).  There are different pools for each geographic region, so pick the one that is local to your area. 
```txt
server 1.north-america.pool.ntp.org
server 2.north-america.pool.ntp.org
server 3.north-america.pool.ntp.org

driftfile /var/log/ntp.drift
```

### Starting (or stopping) nptd (centos 6)
Depending on your system, the start up process can be different.  For Redhat/Centos, the start up process is
```bash
# service ntpd start
```
For Gentoo it's
```bash
# /etc/init.d/ntpd start
```

You can make sure that it is working by checking the running processes, or by simply running the above, but replace "start" with "status". 
```bash
$ ps -ef |grep ntp
root 6648 26033 0 22:05:34 pts/6 0:00 ntpq
root 6637 1 0 22:01:02 ? 0:00 /usr/lib/inet/xntpd
```

If it does not seem to be running, it might be because the `/etc/ntp.conf` file either does not exist, or is incorrect.

### Starting (or stoping) nptd (centos 7)
```bash
[root@dga01 ~]# systemctl start ntpd
[root@dga01 ~]# systemctl enable ntpd
[root@dga01 ~]# systemctl status ntpd
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2018-07-19 18:04:06 EDT; 3min 6s ago
 Main PID: 158269 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─158269 /usr/sbin/ntpd -u ntp:ntp -g

Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: ntp_io: estimated max descriptors: 1024, initial sock... 16
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listen and drop on 0 v4wildcard 0.0.0.0 UDP 123
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listen and drop on 1 v6wildcard :: UDP 123
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listen normally on 2 lo 127.0.0.1 UDP 123
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listen normally on 3 ens817f0 10.36.64.48 UDP 123
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listen normally on 4 lo ::1 UDP 123
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listen normally on 5 ens817f0 fe80::e13a:889f:bdf7:77...123
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: Listening on routing socket on fd #22 for interface updates
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: 0.0.0.0 c016 06 restart
Jul 19 18:04:06 dga01.mb2.prod.variantyx.com ntpd[158269]: 0.0.0.0 c012 02 freq_set kernel -4.321 PPM
Hint: Some lines were ellipsized, use -l to show in full.
[root@dga01 ~]#
```


## ntpq - Diagnostic Tool
After ntpd starts running, you can check how it's doing by using the Network Time Protocol Query tool. When you first startup xnptd, ntpq will show your time as out of sync (or "insane", which has a stratum=16). It takes a few minutes (~20) to get everyone happy and properly synced up.

### Checking to see if ntpd is running
So an initial check with ntpq shows an insane clock:
```bash
$ /usr/sbin/ntpq
ntpq> rv
    
status=c011 sync_alarm, sync_unspec, 1 event, event_restart
system="SunOS", leap=11, stratum=16, rootdelay=0.00,
rootdispersion=0.00, peer=0, refid=0.0.0.0,
reftime=00000000.00000000  Wed, Feb  6 2036 22:28:16.000, poll=4,
clock=c0a577a2.9a09b000  Sun, Jun  2 2002 22:05:38.601, phase=0.000,
freq=0.00, error=0.00
```

But later, you see that everything is synced up nicely.
```bash
$ /usr/sbin/ntpq
ntpq> rv

status=0674 leap_none, sync_ntp, 7 events, event_peer/strat_chg
system="SunOS", leap=00, stratum=2, rootdelay=101.59,
rootdispersion=69.67, peer=33503, refid=NAVOBS1.MIT.EDU,
reftime=c0a582d2.e6db7000  Sun, Jun  2 2002 22:53:22.901, poll=6,
clock=c0a58307.e1d2f000  Sun, Jun  2 2002 22:54:15.882, phase=56.946,
freq=283873.89, error=13.26
```

### Checking external clocks:
You can confirm that you can connect to your external clocks via the ntpq "peers" command. 
```bash
$ /usr/sbin/ntpq
ntpq> peers
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 ns.tx.primate.n 131.243.64.11    2 u   18   64    7   59.951  -686.20  17.621
 barricade.rack9 209.51.161.238   2 u   18   64    7   43.938  -705.36  12.207
 s01.us.it2go.eu 198.60.22.240    2 u   16   64    7   93.759  -713.38  13.321
 LOCAL(0)        .LOCL.          10 l   16   64    7    0.000    0.000   0.002
ntpq>
```

As long as the stratum (`st`) is not `16`, your good!  If it is, check your iptables, or your firewalls. 
 
Also, when `reach = 377`, you have collected [eight good responses](http://support.ntp.org/bin/view/Support/TroubleshootingNTP) and the `*` and `+` are the primary clocks your are getting the timing from

### Checking to see if ntp is multicasting
If your server is setup to multicast out multicast traffic, there are two ways to see if this is running properly. First is to check within ntpq:
```bash
$ ntpq
ntpq> peers
     remote           refid      st t when poll reach   delay   offset    disp
==============================================================================
 NTP.MCAST.NET   0.0.0.0         16 -    -   64    0     0.00    0.000 16000.0
+zorro.sf-bay.or clepsydra.dec.c  2 u  497 1024  377    26.66    3.206    1.31
+bigben.ucsd.edu time.sdsc.edu    2 u  304 1024  377    43.96    0.787    2.27
*NAVOBS1.MIT.EDU .PSC.            1 u  385 1024  377   100.49    7.741    2.96
ntpq>
```

## Sniffing:

### Unicast:
To check to make sure that your system is properly sending and receiving time messages, you can snoop the line and confirm that the messages are going both ways.  You might have to wait a few seconds before you see any traffic as there is not too much that goes over the line. 
```bash
# tcpdump port ntp
16:31:09.146009 IP 10.0.2.15.ntp > barricade.rack911.com.ntp: NTPv4, Client, length 48
16:31:09.189108 IP barricade.rack911.com.ntp > 10.0.2.15.ntp: NTPv4, Server, length 48
16:31:13.146255 IP 10.0.2.15.ntp > ns.tx.primate.net.ntp: NTPv4, Client, length 48
16:31:13.206766 IP ns.tx.primate.net.ntp > 10.0.2.15.ntp: NTPv4, Server, length 48
```

### Multicast:
You can also snoop the interface and see if any multicast packets are leaving.  Note, that they are not constantly broadcasting, so you might have to wait a while before seeing anything.
```bash
# tcpdump multicast

192.168.1.102 -> NTP.MCAST.NET IP  D=224.0.1.1 S=192.168.1.102 LEN=32, ID=68
```

## References:
- [ntp.org](http://ntp.org/): Dr. Mills Time Server web page. It all starts here. (including the headaches)
- [Troubleshooting NTP](http://support.ntp.org/bin/view/Support/TroubleshootingNTP): From the NTP Support page