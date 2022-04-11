# nmap

- [nmap](#nmap)
  - [Overview:](#overview)
  - [Cookbook Examples:](#cookbook-examples)
    - [Ping Scan (fping)](#ping-scan-fping)
    - [Simple Port Scanning:](#simple-port-scanning)
    - [Specific port knocking:](#specific-port-knocking)
    - [OS Detection:](#os-detection)
    - [Output Formats:](#output-formats)
  - [Basic Options Summary:](#basic-options-summary)
  - [References:](#references)

## Overview:
[NMAP](http://nmap.org/) is a very well known port scanning (and stuff) tool.

## Cookbook Examples:
Here's a couple of simple things you can do with nmap:

### Ping Scan (fping)
fping is great, and easy to use, but you can also scan/ping a network with fping:
- `-sP`: ping with ICMP, and tcp:80 (or simple tcp SYN if you don't have rights)
- `-PO#`: protocol ping.  if you only enter "-PO" (with no number) then it pings type 1,2,4.   Replace # with one of the following numbers for the IP types to ping with:
    - `1`: ICMP
    - `2`: IGMP
    - `4`: IP
    - `5`: ST
    - `6`: IPv6

```bash
# nmap -sP 192.168.2.0/24
Starting Nmap 4.50 (http://insecure.org) at 2007-12-28 11:40 EST
Host 192.168.2.1 appears to be up.
Host 192.168.2.3 appears to be up.
Host 192.168.2.4 appears to be up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.281 seconds
```

### Simple Port Scanning:
You can use nmap to tell you all the ports that the servers are listening over.
- `-sS`: use a TCP SYN to knock all ports
- `-sU`: use a UDP empty header to knock all ports
- `-sO`: scan all IP types (IP, icmp, igmp, rdp, tcp, udp, etc...)
- `-sV`: for a specific port/listener, guess what version the listener is (ie: apache 2.1)
- `-sR`: guess RPC versions too

```bash
# nmap -sS 192.168.2.3
Starting Nmap 4.50 (http://insecure.org) at 2007-12-28 09:46 Eastern
Standard Time
Interesting ports on 192.168.2.3:
Not shown: 1707 closed ports
PORT           STATE        SERVICE
135/tcp        open         msrpc
139/tcp        open         netbios-ssn
3389/tcp       open         ms-term-serv
8081/tcp       open         blackice-icecap
Nmap done: 1 IP address (1 host up) scanned in 26.248 seconds
```

### Specific port knocking:
You can test specific ports rather then scanning all of them. 

- `-sT`: scan TCP ports
- `-sU`: use a UDP empty header to knock all ports
- `-p`: scan a specific port (not all)

```bash
# nmap -sU -p 123 66.203.64.10

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2012-04-25 13:26 EDT
Interesting ports on static-10-64-203-66.axsne.net (66.203.64.10):
PORT STATE SERVICE
123/udp open|filtered ntp

Nmap finished: 1 IP address (1 host up) scanned in 0.409 seconds
```

### OS Detection:
Each OS responds to queries a little bit different.  nmap knows those differences, and can give you it's best guess on what kind of os the target boxes have: 
```bash
# nmap -O 192.168.100.2
Starting Nmap 4.50 (http://insecure.org) at 2008-01-03 21:40 EST
Interesting ports on 192.168.100.2:
Not shown: 1709 closed ports
PORT        STATE     SERVICE
631/tcp     open      ipp
1033/tcp    open      netinfo
Device type: general purpose
Running: Apple Mac OS X 10.4.X
OS details: Apple Mac OS X 10.4.8 – 10.4.10 (Tiger) (Darwin 8.8.0 – 8.10.2)
Network Distance: 0 hops
OS detection performed. Please report any incorrect results at http://insecure.
org/nmap/submit/.
Nmap done: 1 IP address (1 host up) scanned in 11.844 seconds
```

### Output Formats:
You don't have to simply ugly up your shell with nmap output.  There are a couple different ways you can output the results.
- `-oG {filename}`: Grepable format.  Good for scripts
- `-oX {filename}`: XML format
- `-oN {filename}`: normal format but to a file
- `-v`: dump all kinds of noise
- `-vv`: even more noise

```bash
# nmap -oG scan.txt 192.168.100.3
# cat scan.txt
# nmap 4.50 scan initiated Sat Jan 5 18:12:34 2008 as: nmap -oG scan.txt
192.168.100.3
Host: 192.168.100.3 (systemB.home.com)    Ports: 135/open/tcp//msrpc///, 139/open/
tcp//netbios-ssn///, 3389/open/tcp//ms-term-serv///, 6346/filtered/tcp//gnutella///,
6347/filtered/tcp//gnutella2///, 8081/open/tcp//blackice-icecap///
# Nmap done at Sat Jan 5 18:12:38 2008 – 1 IP address (1 host up) scanned in
3.810 seconds
```

## Basic Options Summary:
The full (and up to date) [version can be found at the nmap site](http://nmap.org/svn/docs/nmap.usage.txt).  I just marked this up for easier reference.

```txt
Nmap 5.59BETA3 ( http://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports consecutively - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma separted list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --log-errors: Log errors/warnings to the normal-format output file
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (http://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```


## References:
- http://nmap.org/: Nmap Home Page
- Angela Orebaugh; Becky Pinkard (2008). [Nmap in the Enterprise Your Guide to Network Scanning](http://my.safaribooksonline.com/book/networking/network-monitoring/9781597492416).  Syngress