# Installing and working with Cyclades TS Series Console Servers

- [Installing and working with Cyclades TS Series Console Servers](#installing-and-working-with-cyclades-ts-series-console-servers)
  - [Initial Network Config](#initial-network-config)
    - [Clear the old config, and setup network](#clear-the-old-config-and-setup-network)
    - [Set ethernet interfaces speed and duplex](#set-ethernet-interfaces-speed-and-duplex)
    - [modify the pslave.conf file](#modify-the-pslaveconf-file)
  - [Configure The Cyclades](#configure-the-cyclades)
    - [local editing the conf file](#local-editing-the-conf-file)
    - [The pslave.conf file](#the-pslaveconf-file)
    - [command line changes](#command-line-changes)
  - [Appendix 1: References](#appendix-1-references)


## Initial Network Config
When you first get the box, connect to the console, blow away the current config, reconfigure the network, and upload a new image file. By doing this you won't have any odd errors from bad upgrade procedures or any other wackyness. Note though, that we are scrapping the old config. If there is anything worth keeping, make sure you have a saved copy!


### Clear the old config, and setup network
From the console server follow the steps below:

view/clear old config, and reconfigure network.
```
1A01  ! -- view current config if necessary:
1A02  [root@TSx000 /root]# cat /etc/portslave/pslave.conf
1A03  [....ignoring output....]
1A04 
1A05  ! -- Clear the current config:
1A06  [root@TSx000 /root]# echo 0 >/proc/flash/script
1A07  [root@TSx000 /root]# reboot
1A08 
1A09  [....ignoring output....]
1A10  Cyclades-TS2000-Linux V_1.3.12-1 (Aug/26/04)
1A11  Cyclades-TS
1A12
1A13  ! -- enter default username and pass
1A14  TSx000 login: root
1A15  Password: tslinux
1A16  
1A17  ! -- use wiz to configure network
1A18  [root@TSx000 /root]# wiz
1A19  ***************************************************************
1A20  *********** C O N F I G U R A T I O N   W I Z A R D ***********
1A21  ***************************************************************
1A22                                                                                              
1A23  Current configuration:
1A24                                                                                              
1A25  Hostname : TSx000
1A26  DHCP : enabled
1A27  Domain name :
1A28  Primary DNS Server :
1A29  Gateway IP : eth0
1A30  
1A31  Set to defaults? (y/n) [n] : n
1A32  Hostname[TSx000] : netlab-console
1A33  Do you want to use dhcp to automatically assign an IP for
1A34  your system? (y/n) [y] : n
1A35  System IP[] : 10.100.99.5
1A36  Domain name[] : ariba.com
1A37  Primary DNS Server[] : 10.10.10.100
1A38  Gateway IP[] : 10.100.99.1
1A39  Network Mask[#] : 255.255.255.128
1A40  ***************************************************************
1A41  *********** C O N F I G U R A T I O N   W I Z A R D ***********
1A42  ***************************************************************
1A43                                                                                              
1A44  Current configuration:
1A45                                                                                              
1A46  Hostname : netlab-console
1A47  DHCP : disabled
1A48  System IP : 10.100.99.5
1A49  Domain name : ariba.com
1A50  Primary DNS Server : 10.10.10.100
1A51  Gateway IP : 10.100.99.1
1A52  Network Mask : 255.255.255.128
1A53  
1A54  Are all these parameters correct? (y/n) [n] : y
1A55  Do you want to activate your configurations now? (y/n) [y] : y
1A56  Do you want to save your configurations to flash? (y/n) [n] : y
1A57  Thanks for using the Configuration Wizard! Good-bye!
1A58  
1A59  ! -- reboot to let network configs take
1A60  [root@TSx000 /root]# reboot
```

### Set ethernet interfaces speed and duplex
You will probably also want to make sure that your duplexing settings are correct. To set these, use the bootconf command. The following is an example for how to do that.

Setting Ethernet Port Duplex and Speed
```
1B01  ! -- use bootconf to adjust port settings
1B02  [root@TSx000 /root]# bootconf
1B03  Set to defaults (y/n) [N] :
1B04  [...]
1B05  
1B06  MAC address assigned to Ethernet [00:60:2e:00:26:63]:
1B07  IP address assigned to Ethernet interface [10.11.128.254]:
1B08  Watchdog timer ((A)ctive or (I)nactive) [A] :
1B09  Firmware boot from ((F)lash or (N)etwork) [F] :
1B10  Console speed [9600]:
1B11  (P)erform or (S)kip Flash test [P] :
1B12  (S)kip, (Q)uick or (F)ull RAM test [F] : q
1B13  Fast Ethernet ((A)uto Neg, (1)00 BtH, 100 Bt(F), 10 B(t)F, 10 Bt(H)) [F] : F
1B14  Fast Ethernet Maximun Interrupt Events [0]:
1B15  [...]
1B16  
1B17  Do you confirm these changes in flash ( (Y)es, (N)o (Q)uit ) [N] : y
1B18  
1B19  Configuration saved!
1B20  
1B21  fec: Phy @ 0x1, type 0x20005c23
1B22  fec: link DOWN, Configured as: 100 Mbps, Full-Duplex
1B23  fec: Phy @ 0x1, type 0x20005c23
1B24  fec: link DOWN, Configured as: 100 Mbps, Full-Duplex
1B25  fec: link UP, Configured as: 100 Mbps, Full-Duplex
1B26  
1B27  [root@TSx000 /root]# 
```

### modify the pslave.conf file
Now from your local host, scp the download the latest image from cyclades, and scp it to the console server. Make suer that you save the image file with the name xImage rather then what it is when it's downloaded.

uploading the new image
```
1C01  ! -- From your desktop
1C02  chuck $ ls
1C03  zImage_ts_1312-1.bin
1C04  chuck $ scp zImage_ts_1312-1.bin root@10.100.99.5:/proc/flash/zImage

1C05  ! -- then from the console server reboot to take the new os
1C06  [root@TSx000 /root]# reboot
```




## Configure The Cyclades
Some of the other features that will be enabled in this config are:

- (2B045) connect to specific ip address, not tcp port
- (2B023-24, 43) use ssh as the connection protocol (and allow telnet too)
- (2B042, 2C07-12) do not use authentication for accessing console ports. (no need for usernames or passwords)
- (2B104, 113) allow hijacking of sessions.
- () enable snmp logging to a specific host and facility
- (2B050) have a specific login MOTD
- (2B095) set ssh break sequence to "~."
- (2C02) change root password.
- (2C15-16) enable and save changes


### local editing the conf file
Sure, if you want to be a lamer, you can go to the web interface and click a bunch of buttons. Although, if you want better control, I would suggest directly editing the config file.

I think the easiest way to alter this file is on your own box, so I would suggest scping it to your box, editing it, and then scping it back to the console server.


scp config file to and from your local host
```
2A01  ! -- Copy cyclades config files to your local host
2A02  chuck $ scp root@10.100.99.5:/etc/portslave/pslave.conf .
2A03  
2A04  ! -- Edit the file (as per below) 
2A05  
2A06  ! -- Copy marked up config back to the cyclades
2A07  chuck $ scp pslave.conf root@10.100.99.5:/etc/portslave/pslave.conf
2A08  
2A09  ! -- Then enable the changes on the cyclades and save them
2A10  [root@TSx000 /root]# signal_ras hup
2A11  [root@TSx000 /root]# saveconf
```

### The pslave.conf file

In this config we are setting up a console server (CAS), that allows you to directly connect to the servers ports via ssh-ing to a specific IP. The console server will masquerade a series of IP's and give each one to one of its ports, thus if you want to connect to port 25 on the console server, you could ssh to a specific ip (that you could name in dns). and not have to worry about any special tcp ports.

Generic Console Servers /etc/portslave/pslave.conf file
```
2B001  #
2B002  # pslave.conf	Sample server configuration file.
2B003  #
2B004  
2B005  #
2B006  # ----------------------------------------------------------------------- #
2B007  # 1. Server configurations
2B008  #
2B009  
2B010  # ------------------------ #
2B011  # 1.1 Ethernet :
2B012  #
2B013  conf.eth_ip	 10.100.99.5
2B014  conf.eth_mask	 255.255.255.128
2B015  conf.dhcp_client 0
2B016  conf.eth_mtu	 1500
2B017  
2B018  # ------------------------ #
2B019  # 1.2 Other:
2B020  #
2B021  conf.lockdir	/var/lock
2B022  conf.rlogin	/usr/local/bin/rlogin-radius
2B023  conf.telnet	/bin/telnet
2B024  conf.ssh	/bin/ssh
2B025  
2B026  # the parameter syslog (syslog server) was removed from this file 
2B027  # (see the syslog-ng.conf file).
2B028  
2B029  #
2B030  # ----------------------------------------------------------------------- #
2B031  # 2. Console Port Settings: 
2B032  #
2B033  
2B034  # ------------------------ #
2B035  # 2.1 General serial port settings: 
2B036  #
2B037  all.speed	9600
2B038  all.datasize	8
2B039  all.stopbits	1
2B040  all.parity	none
2B041  all.media 	rs232
2B042  all.authtype	none
2B043  all.protocol	socket_ssh
2B044  # IP Address assigned to the serial port.
2B045  all.ipno	10.100.99.16+
2B046  # Maximum reception/transmission unit size for the port
2B047  all.mtu 	1500
2B048  all.mru 	1500
2B049  # Standard message issued on connect.
2B050  all.issue	\r\n\
2B051  *************************************************************************\n\
2B052                              NOTICE:\n\
2B053  This is the netlab console server. The equipment attached to this device\n\
2B054  should not be connected to the general ariba network. Instead, this \n\
2B055  equipment is for testing and learning purposes.  Please record all work\n\
2B056  you are doing, because the changes made today, might be removed tomorrow.\n\
2B057  *************************************************************************\n\
2B058  \r\n
2B059  # Login prompt.
2B060  all.prompt	%h login: 
2B061  # Terminal type, for rlogin/telnet sessions.
2B062  all.term	vt100
2B063  # Linefeed suppression in telnet sessions
2B064  all.lf_suppress	1
2B065  # log logins to the /var/run/utmp file:
2B066  all.sysutmp	1
2B067  all.utmpfrom	"%g:%P.%3.%4"    
2B068  #
2B069  all.flow	soft
2B070  all.dcd	0
2B071  all.DTR_reset	100
2B072  # Inactivity timeout - (time in minutes)
2B073  all.idletimeout 60
2B074  # the parameter syslog_level was removed (see the syslog-ng.conf file)
2B075  # the parameter console_level was removed (see the syslog-ng.conf file)
2B076  
2B077  # ------------------------ #
2B078  # Data buffering:
2B079  #
2B080  #  The number is equal to the buffer size. see /var/run/DB/ttyS#.data
2B081  all.data_buffering	1024
2B082  # DB_mode - Valid only when there is NO session (telnet/ssh/raw) established
2B083  #	to the serial port. (cir for circular)
2B084  all.DB_mode		cir
2B085  # Data Buffer User Logs
2B086  all.DB_user_logs	off
2B087  all.syslog_buffering	0
2B088  all.syslog_sess		0
2B089  all.alarm		0
2B090  all.dont_show_DBmenu 	1
2B091  
2B092  # ------------------------ #
2B093  # ?
2B094  # Send Break to the TTY when this string is received (ssh or telnet). 
2B095  all.break_sequence	~break
2B096  all.break_interval	500
2B097  # Authentication of RADIUS users registered without passwords
2B098  all.radnullpass		1
2B099  
2B100  # ------------------------ #
2B101  # Sniff mode:
2B102  #
2B103  # session mode (in, out, i/o, no)
2B104  all.sniff_mode		in
2B105  # Multiple sessions. 
2B106  # no            - only one sniff session + one normal session are allowed
2B107  #                 (old style). The sniff menu will be presented.
2B108  # yes           - Multiple read/write sessions + multiple sniff sessions
2B109  #                 are allowed. The multiple sniff menu will be presented.
2B110  # RW_session    - read/write sessions will be opened without menu presentation 
2B111  # sniff_session - sniff sessions will be opened without menu presentation 
2B112  # Also, the parameter admin_users must be filled out.
2B113  all.multiple_sessions 	no
2B114  # Escape character (^a,...,^z). This parameter determines which character
2B115  # must be typed to make the session enter into "menu mode". The possible values
2B116  # are <CTRL-a> to <CTRL-z>, and it is only valid for CAS profile.
2B117  all.escape_char 	^z
2B118  all.admin_users		root
2B119  all.multiuser_notif	off
2B120  
2B121  # ------------------------ #
2B122  # Power Management
2B123  #
2B124  # This parameter specifies the Intelligent Power Distribution Unit (IPDU)
2B125  # manufacturer. We currently support IPDU from Cyclades (cyclades), 
2B126  # Server Tech, Inc. (sentry), and BayTech (rpc22). 
2B127  # Each type of IPDU has its own configuration file under the name 
2B128  # /etc/pm.<pmtype>. Make sure that your configuration for all.pmtype matches
2B129  # exactly like the <pmtype> in /etc/pm.<pmtype>. 
2B130  #all.pmtype	cyclades
2B131  #
2B132  # A list of the outlets each user can access. 
2B133  # For example: Joe: 6,3; Helen: 1,3.
2B134  #all.pmusers	
2B135  #
2B136  # List of pairs <ipdu port>.<outlet> where the server is connected. 
2B137  # <ipdu port> is the port number of the ACS/TS where the IPDU is  
2B138  # connected to. <outlet> is the outlet number on the IPDU where the 
2B139  # your server is connected to. 
2B140  # For example: 1.8
2B141  #all.pmoutlet
2B142  #
2B143  # This is the hotkey which causes the switch from the normal socket 
2B144  # telnet/ssh session to the PMD session, and vice-versa. 
2B145  # For example, ^p (Ctrl-p).
2B146  #all.pmkey	^p
2B147  #
2B148  # This is the number of outlets in the IPDU. 
2B149  #all.pmNumOfOutlets	8
2B150  
2B151  
2B152  # ------------------------ #
2B153  # Port-specific parameters
2B154  #
2B155  s1.tty	ttyS1
2B156  s2.tty	ttyS2
2B157  s3.tty	ttyS3
2B158  s4.tty	ttyS4
2B159  s5.tty	ttyS5
2B160  s6.tty	ttyS6
2B161  s7.tty	ttyS7
2B162  s8.tty	ttyS8
2B163  s9.tty	ttyS9
2B164  s10.tty	ttyS10
2B165  s11.tty	ttyS11
2B166  s12.tty	ttyS12
2B167  s13.tty	ttyS13
2B168  s14.tty	ttyS14
2B169  s15.tty	ttyS15
2B170  s16.tty	ttyS16
2B171  s17.tty	ttyS17
2B172  s18.tty	ttyS18
2B173  s19.tty	ttyS19
2B174  s20.tty	ttyS20
2B175  s21.tty	ttyS21
2B176  s22.tty	ttyS22
2B177  s23.tty	ttyS23
2B178  s24.tty	ttyS24
2B179  s25.tty	ttyS25
2B180  s26.tty	ttyS26
2B181  s27.tty	ttyS27
2B182  s28.tty	ttyS28
2B183  s29.tty	ttyS29
2B184  s30.tty	ttyS30
2B185  s31.tty	ttyS31
2B186  s32.tty	ttyS32
2B187  s33.tty	ttyS33
2B188  s34.tty	ttyS34
2B189  s35.tty	ttyS35
2B190  s36.tty	ttyS36
2B191  s37.tty	ttyS37
2B192  s38.tty	ttyS38
2B193  s39.tty	ttyS39
2B194  s40.tty	ttyS40
2B195  s41.tty	ttyS41
2B196  s42.tty	ttyS42
2B197  s43.tty	ttyS43
2B198  s44.tty	ttyS44
2B199  s45.tty	ttyS45
2B200  s46.tty	ttyS46
2B201  s47.tty	ttyS47
2B202  s48.tty	ttyS48
```

### command line changes
Other then making changes to the pslave.conf file, you will also need to modify some other files on the host. The following outlines those steps:

Changes at the console
```
2C01  ! -- change the root password
2C02  [root@netlab-console /root]# passwd
2C03  Enter new UNIX password:
2C04  Retype new UNIX password:
2C05  [root@netlab-console /root]#
2C06  
2C07  ! -- allow non-authenticated ssh connection to the console ports
2C08  ! --   edit the sshd_config file
2C09  [root@netlab-console /root]# vi /etc/ssh/sshd_config
2C10  ! --   and change the PermitEmptyPasswords flag as such
2C11       #PermitEmptyPasswords no
2C12       PermitEmptyPasswords yes
2C13  
2C14  ! -- save changes 
2C15  [root@netlab-console /root]# signal_ras hup
2C16  [root@netlab-console /root]# saveconf
2C17  Checking the configuration file list...
2C18  Compressing configuration files into /tmp/saving_config.tar.gz ... done.
2C19  Saving configuration files to flash ... done.
2C20  [root@netlab-console /root]#
```

## Appendix 1: References
Documents used in creating this document.
- [Cyclades Online Faq](http://www.cyclades.com/support/faqs.php)
- [Cyclades-TS Manual V_1.3.12](bin/Cyclades-TS-Manual-V_1-3-12.pdf).pdf
