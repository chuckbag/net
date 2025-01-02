# limited access user

## Overview:
The challenge is to allow a user to log into the routers and grab the configs, but not be able to make any changes.  This can be very helpful if you have automated tools grabbing configs, but you don't want those accounts to go into the wrong hands and have folks logging in on their own and messing things up.

## Creating user with rights:
The command that lets this happen is the privilege exec command and setting users privilage level to something below 15.

The following commands create a user called "rancid" with a very limited level of rights, but who can view the running config, and view the startup-config.

```
username rancid priv 3 password test
privilege exec all level 3 show running-config
privilege exec level 3 show startup-config
```

## Viewing Configs:
the trick is that the sh run command wont work like normal.  Instead, to view the entire config, you need to enter:

```
show running-config view full
```

Confirming limited rights:
Note that if you wanted to see what commands were available you could enter the ? at the prompt:

```
swa1#?

Exec commands:
  <1-99>           Session number to resume
  access-enable    Create a temporary Access-List entry
  access-profile   Apply user-profile to interface
  clear            Reset functions
  connect          Open a terminal connection
  crypto           Encryption related commands.
  disable          Turn off privileged commands
  disconnect       Disconnect an existing network connection
  enable           Turn on privileged commands
  exit             Exit from the EXEC
  help             Description of the interactive help system
  lock             Lock the terminal
  login            Log in as a particular user
  logout           Exit from the EXEC
  mrinfo           Request neighbor and version information from a multicast
                   router
  mstat            Show statistics after multiple multicast traceroutes
  mtrace           Trace reverse multicast path from destination to source
  name-connection  Name an existing network connection
  ping             Send echo messages
  rcommand         Run command on remote switch
  release          Release a resource
  renew            Renew a resource
  resume           Resume an active network connection
  set              Set system parameter (not config)
  show             Show running system information
  ssh              Open a secure shell client connection
  systat           Display information about terminal lines
  tclquit          Quit Tool Command Language shell
  telnet           Open a telnet connection
  terminal         Set terminal line parameters
  traceroute       Trace route to destination
  tunnel           Open a tunnel connection
  where            List active connections

swa1#
```

and running the simple sh run command would produce almost nothing by itself. 

```
swa1#sh run
Building configuration...
Current configuration : 192 bytes
!
! Last configuration change at 16:08:58 UTC Fri Nov 11 2011 by chuck
! NVRAM config last updated at 21:27:54 UTC Wed Nov 9 2011 by chuck
!
boot-start-marker
boot-end-marker
!
!
!
!
!
!
end

swa1#
```

## Notes/Issues:
This does not work for all os's. For example:

```
Cisco IOS Software, C3560 Software (C3560-IPSERVICESK9-M), Version 12.2(58)SE2, RELEASE SOFTWARE (fc1)

swa2(config)#privilege exec ?
    all     All suboption will be set to the samelevel
    level   Set privilege level of command
    reset   Reset privilege level of command

Cisco IOS Software, C3560E Software (C3560E-UNIVERSALK9-M), Version 12.2(55)SE3, RELEASE SOFTWARE (fc1)

swa3(config)#privilege exec ?
    level   Set privilege level of command
    reset   Reset privilege level of command
```

## References:
- [Allow user view Running/Startup-Config (red-only) in Cisco IOS](http://www.itsyourip.com/cisco/allow-user-view-runningstartup-config-red-only-in-cisco-ios/)
- [Configuring Passwords and Privileges](http://www.cisco.com/en/US/docs/ios/12_2/security/configuration/guide/scfpass.html): Cisco IOS Security Configuration Guide Release 12.2
- [Using the Command Line Interface](http://www.cisco.com/en/US/docs/ios/12_0/configfun/configuration/guide/fcui.html): Cisco IOS Release 12.0 Configuration Fundamentals Configuration Guide