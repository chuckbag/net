# Initial Login

- [Initial Login](#initial-login)
  - [Overview](#overview)
  - [Console Access:](#console-access)
  - [Initial Login:](#initial-login-1)
  - [Getting into Edit Mode:](#getting-into-edit-mode)
  - [Getting back to the Unix Shell:](#getting-back-to-the-unix-shell)
  - [Being Able to Save:](#being-able-to-save)
  - [Saving:](#saving)
  - [References:](#references)

(note, this example is with a J-Series Router)

## Overview
In this doc, we're really not configuring the junos device, we're just logging in and learning to save.  

## Console Access: 
From the RS-232 port, use a [standard Straight Cable](../../Other/cableWiring/README.md) (and Straight RJ45-DB9 adapter if needed), and set your modem settings to 9600/N/8.

## Initial Login: 
When you connect, you get the following prompt.  
```
Amnesiac (ttyd0)

login:
```

Login with user = `root`, and a blank password.  
```bash
login: root


--- JUNOS 10.2R3.10 built 2010-10-16 19:20:24 UTC

********************************************************************
** Welcome to JUNOS:                                              **
**                                                                **
**     To run the console configuration wizard, please run the    **
**     command 'config-wizard' at the 'root%' prompt.             **
**                                                                **
**     To enter the JUNOS CLI, please run the command 'cli'.      **
**                                                                **
********************************************************************
Amnesiac (ttyd0)

root@%
```

## Getting into Edit Mode: 
You start off in the unix shell of the router.  
```bash
root@%
```

This is a helpful thing as down the road, via the shell, you can collect syslogs, and do other unix things directly to the router.  

To get to the standard command line of the router (the CLI) enter the command cli: 
```bash
root@% cli
root> 
```

Obviously you want to start configuring your brand new device.  To do this enter into the edit mode to start whacking away!
```bash
root> edit
Entering configuration mode
 
[edit]
root#
```

## Getting back to the Unix Shell: 
you can break back out of cli via the start shell command: 
```bash
cmercier@atl-fw1b01> start shell
% cd /
% ls -lF
total 177
-rw-r--r--   1 root  wheel   6187 Nov 24  2011 COPYRIGHT
dr-xr-xr-x   2 root  wheel   2048 Nov 24  2011 a/
dr-xr-xr-x   2 root  wheel   2048 Nov 24  2011 altconfig/
dr-xr-xr-x   2 root  wheel   2048 Nov 24  2011 altroot/
dr-xr-xr-x   2 root  wheel   2048 Nov 24  2011 b/
dr-xr-xr-x   2 root  wheel   4096 Nov 24  2011 bin/
...
```

## Being Able to Save: 
JunOS really does not like not having a password set, so make that change first.  

To set the system password enter in the following system command, and then at the prompts, enter in the password: 
```bash
root# set system root-authentication plain-text-password
New password:  *******
Retype new password:  *******

[edit]
root#
```

## Saving: 
Unlike some systems, in JunOS, the change that you entered does not take effect.  You need to commit (save) that change before it works.  

First, its a good idea to review the config that you just made, to ensure that it was done right, and that there is not anything else in there.  
```bash
root# show | compare
[edit system]
+   root-authentication {
+       encrypted-password "$1$YIcniIWf$sN.gW2LkIgrYP0wXL9nyP1"; ## SECRET-DATA
+   }

[edit]
root# 
```

Next it's a good idea to confirm that the changes code is correct.  The Check command scans the code to commit, and makes sure that it is compatable with the current running hardware/software, and if there are any bugs with it.  
```bash
root# commit check
configuration check succeeds

[edit]
root# 
```

Now you can save the change.  It's smart to get used to the commit confirmed command to do this, becayse it allows you to make the changes, but easily revert them if you make a change that will blow you out of the box for any reason.  
```bash
root# commit confirmed
commit confirmed will be automatically rolled back in 10 minutes unless confirmed
commit complete

# commit confirmed will be rolled back in 10 minutes
[edit]
root# 
```

To finalize the commit, enter the commit command again.  You can append the and-quit command to leave the edit mode.  
```bash
root# commit and-quit
commit complete
Exiting configuration mode

root>
```

## References: 
- [How to setup a serial Console connection to the J-series communications port using HyperTerminal](http://kb.juniper.net/InfoCenter/index?page=content&id=KB10116): Juniper KB10116, 29 Jun 2010
- [Initial / default login name and password on EX-series switches](http://kb.juniper.net/InfoCenter/index?page=content&id=KB11471&cat=JUNOS&actp=LIST): Juniper KB11471, 11 Aug 2010
