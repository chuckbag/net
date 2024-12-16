# IOS Configs

## House Cleaning: 
- [Upgrading IOS on Cisco Routers](upgrading-ios-on-cisco-routers.md): A writeup on the steps necessary to upgrade routers OS images.
- [@#$%@! enable passwords](enable-passwords.md): Cisco - Password Recovery Procedure for the Cisco 3600 Series Routers. This comes in handy when inheriting a router from someone else and not getting it's enable password.
- [Cleaning the configs of a Cisco Router](virgin-ise-a-cisco-router.md): to "virgin-ize" a router.
- virgin-izing a device: Steps you need to take to completely clean out a ios device: 
- upgrading the os: what to do to get a newer version running on your box.
- Moving files around a box/stack: It's almost like unix, but not exactly

## Basic Configs: 
- Starting Fresh with a 3500: Resetting the Password, Virginizing the config, and Upgrading the OS.
- User accounts and passwords: how to properly encrypt a password, and how to enable ssh access
- Trunking and Port Channels: bonding vlans within a single interface and bonding multiple interfaces together
- Ports, Trunks and Port-Channels: How to configure and modify (and limit VLANs)
- Enabling routing or default routes: How to setup a def route on a SVI, or how to enable routing on the switch/router.
- Sniffing (span) on IOS: how to sniff traffic on a ios switch. 
- Setting up SNMP: how to enable snmp on a switch, and what to monitor
- Syslog: how to send log messages to a remote syslog server
- Logging to local and Syslog servers: how to track the status of the device

## Routing: 
- HSRP: (like vrrp, but cisco only)
- BGP: how to connect to the internet

## Advanced Features: 
- Security Additions: List of additions to the standard config that make the box more secure.
- Layer 3 QoS.  In the routing world, it's about slowing down everyone.
- dhcp forwarder: how to forward layer-2 dhcp packets over a layer 3 network.
- DHCP Server: How to have the router provide dhcp services.  
- bridging vlans: How to install a wan accelerator logically rather then having to physically install it between devices in your network.  
- limited access user: Allow monitoring tools to grab configs but not make changes (ie:rancid)
- AAA + ISDN + PPP: Configuring ISDN dialup with console and PPP, and with AAA (radius) authentication.

## Troubleshooting:
- Troubleshooting Switch Port and Interface Problems: Good commands to use, and how to interpret them (ie: what all the var's are for sh int)
- spanning tree: 
- Simple port troubleshooting: Why can't you pass traffic on the new port? 

## Hardware Issues: 
- 3750 Stacking Issues: Rules and processes for dealing with the stackable switches. 

## Automation:
- Cisco 3120 (HP Blade Switch) Config Builder:Quick way to make sure that your configs are correct, and with only a few typos.

## References: 
- Cisco IOS Command Reference Master Index for IOS versions: 12.0, 12.1, 12.2
- IOS Naming Conventions: Look at an IOS software image file name, and decode what it is.
- Project DOTU and Undocumented IOS: Listing all the undocumented Cisco Commands and features.
- IOS Password Encryption Facts: why to use "enable secret" and not user passwords
- Improving Security on the Cisco Routers
- Characterizing and Tracing Packet Floods Using Cisco Routers
