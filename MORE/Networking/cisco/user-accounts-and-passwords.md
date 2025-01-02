# User accounts and passwords

- [User accounts and passwords](#user-accounts-and-passwords)
  - [Overview:](#overview)
  - [Setting up the Environment](#setting-up-the-environment)
    - [Enabling Local Accounts:](#enabling-local-accounts)
    - [Encrypting all passwords in the configs](#encrypting-all-passwords-in-the-configs)
    - [Enable Password:](#enable-password)
  - [Accounts:](#accounts)
    - [Creating a User Account](#creating-a-user-account)
    - [Copying/Pasting Encrypted Passwords:](#copyingpasting-encrypted-passwords)
    - [Dangers of Crackable Passwords:](#dangers-of-crackable-passwords)
  - [Enabling Access to the System:](#enabling-access-to-the-system)
    - [Configuring Terminal and Console Access:](#configuring-terminal-and-console-access)
    - [Enabling SSH Access:](#enabling-ssh-access)
  - [References:](#references)



## Overview:

## Setting up the Environment
Here we go over all the things you need to have working to be able to get user accounts working: 

### Enabling Local Accounts: 
Accounts can be stored on the local device, or accounts can be managed via a Radius server.  if you want to store the accounts locally, make sure you enable aaa, To do this do the following: 

```
aaa new-model
aaa authentication login default local
aaa authorization exec default local
```

### Encrypting all passwords in the configs
To enable encryption of all passwords in the saved configs, you need to enable the global password-encryption command. 

```
conf t
  service password-encryption
end
```

To show how this would work, here we see the configs before password encryption: 

```
[...]
username rancid privilege 3 password 0 test
[...]
```

And then after the command is entered, how the password is encrypted into an encrypted string 

```
[...]
username rancid privilege 3 password 7 051F031C35
[...]
```

### Enable Password:
If a user account IS NOT set to privilege level 15, then they will need to enable, (or "su") to a level that will allow them to make changes to the system.  

To create an enable password, and have the password encrypted in the config file enter the following: 

```
conf t
  enable secret 0 BigBooTay
end
```

this will result in something like this in your running config

```
enable secret 5 $1$2ASJ$aSefGi55hhFb3Ro6TQA3P1
```

Note that there are three main levels of privilege levels. Privilege level 15 provides full rights to all commands. All other levels need to enable to get all the commands available.  
- privilege level 0 — Includes the disable, enable, exit, help, and logout commands.
- privilege level 1 — Normal level on Telnet; includes all user-level commands at the router> prompt.
- privilege level 15 — Includes all enable-level commands at the router# prompt.

You can allow users with lower privilege levels to use upper level commands by default by "redefining" commands privilege levels.  An example to this would be the following:  

```
privilege configure level 8 snmp-server community 
privilege exec level 6 show running 
privilege exec level 8 configure terminal
```
 

##  Accounts:
### Creating a User Account
The `username` command allows you to create accounts, set the privilege level, and set the users password.  To create a new user, you would simply enter the following:

```
conf t
  username rancid privilege 3 password 0 test
end
```

Where the username is "rancid", the level is "3" (which is a very limited access account), and the password is entered in plaintext "0" and it is "test".  

### Copying/Pasting Encrypted Passwords:
If you want to copy the configs from one router to the other, you can copy the encrypted password from one to the other and it will simply work.  (they all use the same encryption algorithm.)  Also, the config says if the password is encrypted, in this example, the "5" tells the router that the following command being entered is already encrypted, and to no re-encrypt it.  

```
[...]
username rancid privilege 3 password 5 051F031C35
[...]
```

### Dangers of Crackable Passwords: 
Make sure you **don't use** `enable password`, or use the encryption level `7`.  Both of these use simple hashing, and are easily cracked.  

As an example, the following links to a Cisco Type 7 Reverser tool to crack passwords.  

http://packetlife.net/toolbox/type7/

## Enabling Access to the System: 

### Configuring Terminal and Console Access:
To allow connectivity to the system, you need to configure the vty settings.  There are not an unlimited amount of terminals (or number of ssh connections) that can be made at a given time.  By default we enable 5 sessions, "0" through "5".  You also want to make sure that you enable an ACL on the terminal, to control who can connect.  

First configure the ACL: 

```
ip access-list standard vty-access
 remark CLI accesss from mgmt networks
 permit 10.50.32.0 0.0.0.255
 permit 10.50.34.0 0.0.0.255
 permit 10.33.195.0 0.0.0.255
 permit 10.33.0.0 0.0.255.255
 deny   any log
 exit
!
```

Then configure the five terminals: 

```
line vty 0 4
 access-class vty-access in
 exec-timeout 30 0
 login local
 transport input ssh
 exit
!
```

Note that this allows ssh inputs and uses "local" accounts rather then radius or something.

### Enabling SSH Access: 
If it was not setup already, to enable crypto on the device you need to first define the dns domain

```
ip domain-name cmed.us
```

and then you need to generate an ssh key

```
crypto key generate rsa
ip ssh time-out 60
ip ssh authentication-retries 2
```


## References:
- [IOS Privilege Levels Cannot See Complete Running Configuration](http://www.cisco.com/en/US/tech/tk59/technologies_tech_note09186a00800949d5.shtml). Cisco Press Doc 23383, Jan 14, 2008
- [Configuring Secure Shell on Routers and Switches Running Cisco IOS](http://www.cisco.com/en/US/tech/tk583/tk617/technologies_tech_note09186a00800949e2.shtml): Doc 4145, Jun 28, 2007
- [Cisco IOS Password Encryption Facts](http://www.cisco.com/c/en/us/support/docs/security-vpn/remote-authentication-dial-user-service-radius/107614-64.html): Doc 107614, July 21, 2008