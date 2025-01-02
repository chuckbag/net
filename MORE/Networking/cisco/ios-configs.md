# IOS Configs

## House Cleaning: 
- [Upgrading IOS on Cisco Routers](upgrading-ios-on-cisco-routers.md): A writeup on the steps necessary to upgrade routers OS images.
- [@#$%@! enable passwords](enable-passwords.md): Cisco - Password Recovery Procedure for the Cisco 3600 Series Routers. This comes in handy when inheriting a router from someone else and not getting it's enable password.
- [Cleaning the configs of a Cisco Router](virgin-ise-a-cisco-router.md): to "virgin-ize" a router.
- [virgin-izing a device](virgin-ise-a-cisco-router.md): Steps you need to take to completely clean out a ios device: 
- [upgrading the os](upgrading-the-os.md): what to do to get a newer version running on your box.
- [Moving files around a box/stack](moving-files-around-a-boxstack.md): It's almost like unix, but not exactly

## Basic Configs: 
- [Starting Fresh with a 3500](starting-fresh-with-a-3500.md): Resetting the Password, Virginizing the config, and Upgrading the OS.
- [User accounts and passwords](user-accounts-and-passwords.md): how to properly encrypt a password, and how to enable ssh access
- [Trunking and Port Channels](trunking-and-port-channels.md): bonding vlans within a single interface and bonding multiple interfaces together
- [Ports, Trunks and Port-Channels](ports-trunks-and-port-channels.md): How to configure and modify (and limit VLANs)
- [Enabling routing or default routes](enabling-routing-or-default-routes.md): How to setup a def route on a SVI, or how to enable routing on the switch/router.
- [Sniffing (span) on IOS](sniffing-span-on-ios.md): how to sniff traffic on a ios switch. 
- [Setting up SNMP](setting-up-snmp.md): how to enable snmp on a switch, and what to monitor
- [Syslog](syslog.md): how to send log messages to a remote syslog server
- [Logging to local and Syslog servers](logging-to-local-and-syslog-servers.md): how to track the status of the device

## Routing: 
- [HSRP](hsrp-and-weighted-static-routes.md): (like vrrp, but cisco only)
- [BGP](bgp.md): how to connect to the internet

## Advanced Features: 
- [Security Additions](security-additions.md): List of additions to the standard config that make the box more secure.
- [Layer 3 QoS](layer3-qos.md).  In the routing world, it's about slowing down everyone.
- [dhcp forwarder](broadcast-forwarder.md): how to forward layer-2 dhcp packets over a layer 3 network.
- [DHCP Server](dhcp-server.md): How to have the router provide dhcp services.  
- [bridging vlans](bridging-vlans.md): How to install a wan accelerator logically rather then having to physically install it between devices in your network.  
- [limited access user](limited-access-user.md): Allow monitoring tools to grab configs but not make changes (ie:rancid)
- [AAA + ISDN + PPP](aaa-isdn-ppp.md): Configuring ISDN dialup with console and PPP, and with AAA (radius) authentication.

## Troubleshooting:
- [Troubleshooting Switch Port and Interface Problems](http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015bfd6.shtml): Good commands to use, and how to interpret them (ie: what all the var's are for `sh int`)
- [spanning tree](spanning-tree-troubleshooting.md): 
- [Simple port troubleshooting](simple-port-troubleshooting.md): Why can't you pass traffic on the new port? 

## Hardware Issues: 
- [3750 Stacking Issues](3750-stacking-switches.md): Rules and processes for dealing with the stackable switches. 

## Automation:
- [Cisco 3120 (HP Blade Switch) Config Builder](bin/Cisco3120xBuilder.xlsx): If you need to make many duplicate switch configs, and you don't want to write a perl/python script, you can easily use excel and paste the output to txt, and run a few "replace all" commands.

