# Unix / Linux




[<img src="img/linux-tools.jpeg" width="300">](img/linux-tools.jpeg)

## Tools 
DNS
- [dns](): What it is and how to set it up.
- [dnstracer](): view the registar and what roots resolve
- [dig](): like nslookup only better

ICMP
- [fping](fping.md) is a pinging utility that will sweep ip ranges.
- [mtr](http://www.bitwizard.nl/mtr/): pinging and traceroute

Site Analysis
- [nmap](): Scan IP/Ports on a network
- [zmap](): Scan IP/Ports on a network 
- [ipcalc](): compute network ranges around IP/Masks
- [netperf](): send traffic between two hosts, and review the behavior of the network
- [tcpdump](): capture packets seen by your machine
- [dummynet](): WAN traffic emulation tool
- [watch](): View via cli interface traffic
- [ntopng](): IP Flow for your hosts

Shell Scripts: 
- [Perl](../../perl/README.md): 
- [Python](../../python/README.md): 
- [Java](../../Java/README.md):
- [Ruby](../../Ruby/README.md): 
- [RegEx](../../Other/regexp.md): 
- [Bash](bash/README.md): 
- [sed/awk](sedawk.md): Auuughhh!!!
- [10 Tools To Add Some Spice To Your UNIX Shell Scripts](http://www.cyberciti.biz/tips/spice-up-your-unix-linux-shell-scripts.html).  by Vivek Git

## System Deployments:
- Gentoo Systems: a bare-bones linux distro that is really nice to work with
- Centos Systems: A free version of redhat
- RedHat Systems: The commercial linux distribution
- Brew: Mac Linux

## VM Services:
- Oracle VM VirtualBox: A vm server for windows or linux.
- KVM: running under centos
- HP Blade Management: Commands you can use to view status of blades/chassis

## Services

Revision Control
- [git](): A good revision control system for code.  (does not deal with larger binaries very well.) 
- [subversion](): Not as good as git for some things, but holds binaries better. 

File Transfers and Manipulation: 
- [rsync](rsync.md): A fantastic copy tool.
- [sshpass](sshpass.md): How to run ssh commands without getting prompted for a pass
- [tftp](tftp.md): The steps for setting up a TFTP daemon on your server.
- [wget](wget.md): Allows you to backup content from web or FTP sites
- [curl](curl.md): just like wget
- [tar+gz](targz.md): how to stuff a lot of files into one zipped up one.  
- [ztools](ztools.md): different commands for working with compressed (.gz) files
- [nfs](nfs.md): mounting remote volumes locally
- [using Tapes (LTO)](using-tapes-lto.md): how to get a tape storage device working

Shell level stuff: 
- [bash](bash/README.md): Your shell and how to modify it. 
- [fancy shell commands](http://www.commandlinefu.com/commands/browse/sort-by-votes): Things you didn't know you could do.
- [screen](screen.md): multiple sessions on one window.  Also helps with disconnecting wan links.
- [grep](grep.md): few tricks to finding stuff
- [df and du](du-and-df.md): looking at disk and directory usage
- [Diffing tools](diffing-tools.md): A couple of them out there...
- [sudo](sudo.md): allowing folks to do rooty stuff

Security: 
- [iptables](): edge firewalling
- [SELinux](): To the Death!  No! To the Pain!

Other: 
- [snmp](snmp.md): polling and alerting tool
- [ntpd](): Setting up your box to host or query the correct time.  
- [nload](): view network interface traffic
- UNetbootin allows you to create bootable Live USB drives for Ubuntu, Fedora, and other Linux distributions without burning a CD. It runs on both Windows and Linux.
- Infra Recorder: a free opensource Image/CD Burning program
- Network monitoring on Linux: 

Services: 
- [cron](): running scripts at specific times
- [syslog](): local and remote logging
- [syslog-ng](): How to setup a syslog service
- Apache: how to setup and debug
- [ansible](): central management of everything!
- [rsnapshot](): never tarball your backups again!
- [tftp server]()
- cacti install on Centos, and how to fully deploy cacti:
- [git repo](): so you can backup your files
- PXE/Jumpstart server: get all your other boxes booted easily
- BIND9, or BIND9-Chrooted: dns
