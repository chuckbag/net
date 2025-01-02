# 3750 Stacking Switches

- [3750 Stacking Switches](#3750-stacking-switches)
  - [3750 Stack software imaging Summary:](#3750-stack-software-imaging-summary)
  - [Stacking the 3750s:](#stacking-the-3750s)
  - [3750 Config management:](#3750-config-management)
  - [3750 Stack Management:](#3750-stack-management)
    - [When building a stack:](#when-building-a-stack)
    - [After removing a switch from a stack:](#after-removing-a-switch-from-a-stack)
    - [Active Console Ports:](#active-console-ports)
  - [Software Management on a single 3750:](#software-management-on-a-single-3750)
  - [Software Management on Multiple 3750s:](#software-management-on-multiple-3750s)
  - [References:](#references)


## 3750 Stack software imaging Summary:
The 3750 stack platform offers a unified management interface for multiple switches, reducing management overhead. The overall configuration of the 3750 stack behaves in a fashion like a unified chassis, such as a 6500 series switch. The 3750s are each autonomous systems, however, and issues can arise when trying to replace or insert new switches in an existing stack. The following is meant to act as a brief guide to understanding this behavior, and is intended to be slightly more concise than the cisco technical documentation.

## Stacking the 3750s:
Each 3750 comes with a stackwise cable meant to be connected to the two stacking ports on the back of the switch. It is essential that when a stack is created, a full loop is created with stackwise cables. Having a full loop doubles the throughput of the stackwise connections, as well as provides redundancy in a situation where a switch fails and is removed from the loop.

There are multiple ways to accomplish this, but a consistent “Switch 1 (Stackwise 1) -> Switch2 (Stackwise 2) ” cabling scheme is easiest to manage. The last unit in the stack should then be cabled to the open port on the first unit of the stack. Due to stackwise length limitations, all 3750s should all be in the same rack, directly above/below each other.

## 3750 Config management:
In its simplest configuration, a 3750 stack consists of a switch called the “Stack master” and member switches. The stack master is the switch responsible for maintaining the authoritative configuration for the stack. The stack master periodically copies this config file to other member switches, so in event of master failure, another switch is eligible to maintain the integrity of the stack. The stack Master is chosen with a variety of inputs, the most deterministic of which is the command :

```
Switch [#] priority [1 – 15]
```

All switches have a default priority of 1. Higher priorities win master elections, so it is a good practice to set a known switch as the stack master.

Elections only occur when a switch is added to a stack. The following two scenarios may result in unexpected behavior, so it is suggested to never expand a stack in these manners:
- Do NOT add a running switch into a live stack.
- Do NOT add a new switch to a powered down stack.
- In both of these situations, it is possible to inadvertently lose the original stack configuration and put the stack into an “unknown state” if the new switch wins the master election.
- It is best practice to add the switches in the following manner:
- Break the stackwise link between the first and last members of the live stack.
- Cable the new switch into the live stack via the StackWise cables, ensuring that there is a full loop once all the connections are made.
- Power on the new switch.

When these procedures are followed, the new switch will be brought online with a default configuration, and the existing stack will not experience any downtime. If it is desired to pre-configure the new switch, this is accomplished in the stack prior to insertion, as explained later.

Factors that affect stack master elections include (In descending order of priority):

- The switch that is currently stack master
- The switch with the highest priority
- Switch with non-default config
- Software version
- Longest uptime
- Lowest MAC address

## 3750 Stack Management:
Assuming two bare metal 3750s are stacked together, the winner of the Stack Master election becomes switch #1, and the next switch is #2. New switches inserted into a stack become numbered consecutively higher. The easiest way to control this election process is to power on the desired master switch first, then power on switch #2 after a 20-30 second delay delay, and proceed through the stack in this manner. When a switch is added to a stack, a line is inserted near the top of the config corresponding to its position in the stack, and its hardware type.

```
Switch 1 provision ws-c3750e-pd
Switch 2 provision ws-c3750e-pd
```

Once a switch is ‘provisioned’ in the stack, the stack master will now accept port-level configurations for the expected switch. It is possible to manually provision a stack to expect a new member. In this fashion, you can pre-configure the stack when new hardware is anticipated. When a new switch of the corresponding hardware type is inserted in the stack, it picks up the pre-existing configuration.

When a 3750 is added to a stack, the stack master determines if there are any provisioned switches that are not currently live in the stack.

If there is a switch configuration provisioned that matches the number assigned to the new switch, the new switch assumes that number in the stack, and that configuration.

If the new switch has the same number as an existing live switch member in the stack, the new switch assumes the lowest provisioned available number in the stack, and any config in the master switch is applied to the newly provisioned switches.

If there are no more provisioned switches in the stack, the newly inserted switch will retain its existing number, if this number does not already exist in the stack. If the number configured on the switch exists in the stack, the switch will assume the lowest available number. The configuration of ports on this switch will be set to defaults.

Once a switch is “numbered”, it retains that number for the duration of its life, even when it’s removed from a stack. In order to change the numbering of the switch and all of its ports back to 1, it is necessary to configure this manually, with the command:

```
Switch # renumber #
```

It is possible to renumber a switch while it is in a stack, but you cannot change it to a number that is currently active in the stack.

When a switch is removed from a stack, it also retains all the provisioned information for the former members of the stack- it is necessary to unprovision the former stack members:

```
No switch # provision
```

With these parameters in mind, the following best practices should be kept in mind:

### When building a stack:

Boot the switch you desire to be the stack master, and configure the switch priority as 15 (as the higher the priority number wins the master election).

```
Switch 1 priority 15
```

### After removing a switch from a stack:

```
Write erase
Switch # renumber 1
reload
```

### Active Console Ports:
All console ports in a 3750 are active. Issuing a “who” command indicates that line console 0 of the stack is bound to the master switch. The other console ports appear to connect to the master switch via an vty internal telnet session as indicated below:

```
Line User Host(s) Idle Location
   0 con  0       idle 00:00:40
 * 1 vty  0       idle 00:00:00 127.0.0.4
   2 vty  1       idle 00:02:02 127.0.0.5
```

## Software Management on a single 3750:
The software available for a 3750 consists of two options through cisco- the raw .bin file, or a .tar file structure that includes not only the .bin file, but a directory hierarchy and files that are intended to be used with the web-based device manager. Working with the .bin files is fairly straight forward- copy the new .bin file to the switch, set the boot statement in config mode, and reboot. This is very similar to the process in place with other cisco devices.

Working with the .tar file is also an easy process- the “archive” command will obtain, decompress, and upgrade the switch relatively painlessly. There are certain parameters available to force a reload after the successful upgrade, as well as either delete the old image or keep it until a successful upgrade has been completed.

The syntax looks as follows:

```
archive download-sw /safe tftp://x.x.x.x/c3750-xxxxxx.tar
```

This would require the engineer to reboot the system after the archive has completed. This is approximately a 10 minute process, largely done without much output to the console- be patient. There are additional flags available that force a reload, or leave the software on the switch entirely.

If you want to pull an os from one switch in a stack to another, so that they all have the same running os, do the following to copy the os from switch 1 TO switch 2: : 

```
archive copy-sw /force-reload /overwrite /dest 2 1
reload
```

## Software Management on Multiple 3750s:
Because each 3750 is essentially an autonomous system, it is possible to stack 3750s with differing versions of software. When a switch with a different revision of software from the stack master is inserted into the stack, the new switch comes up in “version mismatch” mode. By default, the 3750 master will now attempt to auto-update the software version on the new stack member, and perform an upgrade or downgrade as necessary. This auto-update functionality requires the existence of the .tar file structure. I was unable to follow documentation on cisco’s web site and successfully manually update a software-mismatched stack member using a .bin file.

The steps that cisco outlines are as follows:

```
copy the image file from the host machine to the new machine, using:
copy flash#:/filename.bin flash#/filename.bin
conf t
boot sys switch # flash:/filename.bin
exit
reload slot #
```

This series of commands successfully copies the file from the host system to the new system, but was unsuccessful at updating the boot parameter on the target system.

As a result, it was almost necessary to remove the new switch from the stack, perform the upgrade manually, and then re-insert it into the stack.

It is possible to also update the software on a live stack using just the .bin files, but it is necessary to copy the files and set all the boot statements on each member switch individually.

The Autoupdate functionality in a stack relies upon the existence of the .tar file and associated directory structure created by the ‘archive’ function. As a result, it may be best practice to use the .tar file for updating software within a stack, even though the .tar files contains extra files that will not be utilized in our environment.

Updating the software on a master via the archive process automatically updates software on every member of the stack.

## References: 
- [Catalyst 3750 Software Upgrade in a Stack Configuration with Use of the Command-Line Interface](http://www.cisco.com/en/US/products/hw/switches/ps5023/products_configuration_example09186a00804799d7.shtml)
- [Creation and Management of Catalyst 3750 Switch Stacks](http://www.cisco.com/en/US/products/hw/switches/ps5023/products_configuration_example09186a00807811ad.shtml#stack3)
- [Managing Switch Stacks](http://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst3750x_3560x/software/release/12-2_55_se/configuration/guide/3750xscg/swstack.html):