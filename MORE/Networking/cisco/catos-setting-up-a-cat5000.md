# Setting up a cat5000

## Initial Setup
Note that the switch comes out of the box with no telnet configured. All settings must be manually done thought the serial cable which is a standard Cat5 Patch cable connected to a DB9-RJ45 converter which attaches to your computer.


### Set Switch IP Configuration
Here we just setup the basic configuration so the switch can be telneted into.

Set the management interface sc0 to have a specific ip and netmask

```
set interface sc0 ip_addr netmask
```

Set the management interface sc0, to a specific vlan. Note, this will not work unless you have already setup the VLAN's. If you have not, you can wait and do this later.

```
set interface sc0 vlan_num
```

This just sets the default gateway, but the command can be modified by removing the "default" command so that it can route to specific networks.

```
set ip route default gateway
```

These two commands shows the settings that were just set.

```
show interface
show ip route
```

### Set Switch Ports
This is just the basic port configs, not including setting up VLAN's.

To set the speed of the port. Statically assigning speeds up a ports initial time it takes to setup the port.

```
set port speed mod_num/port_num {10 | 100 | auto}
```


To set the duplex mode. Note that this must be set the same on both sides.
```
set port duplex mod_num/port_num {full | half}
```


On Fast E or Gig E ports, set the flow control mode for transmit and receive ports, and set the config link negotiation.

```
set port flowcontrol mod_num/port_num {recieve | send} {on | off | desired}
```

You can set the name for each port

```
set port name mod_num/port_num name_string
show port mod_num/port_num
```


### Set VLAN Trunk Protocol
For configuring the Switch as a VTP Server:

```
set vtp domain name
set vtp mode server
show vtp domain
```


## Working with VLANS

### Create VLANs

```
set vlan vlan_num [name name]
show vlan vlan_num
```

### Assign Switch Ports to VLANs

```
set vlan vlan_num mod_num/port_num
show vlan vlan_num
show port [mod_num/port_num]
```

### Configure VLAN Trunks

```
show port capabilities [mod_num[/port_num]]
set trunk mod_num/port_num {on | desirable | auto} {isl | dot1q | negotiate}
show trunk
```

## Administering the Switch
This is laid out below the section "Working with VLAN's" because some of these steps involve having setup the vlans first.

### Global System Settings

```
set system name name_string
set system location location_string
set system contact contact_string
set time mm/dd/yy hh:mm:ss
set prompt prompt_string
set password
set enablepass
set banner motd c [text] c
```

Where c is a character string that signifies the beginning and ending of the message

### Manage the Configs

```
show config
copy config tftp
write network
copy tftp config
configure network
```

## Monitoring the Switch

### General

```
show system
show module [mod_num]
```

### Ports

```
show port [mod_num[/port_num]]
show port capabilities [mod_num[/port_num]]
```

### Network

```
ping [-s] host [packet_size] [packet_count]
traceroute [-q nqueries ] data_size
show interface
show ip route
```

## Setting Up Sysloging

```
pepe> (enable) set logging server 10.7.128.51 10.7.128.51 added to System logging server table.  
pepe> (enable) set logging server facility local3 System logging server facility set to   
pepe> (enable) set logging server severity 5 System logging server severity set to <5>  
pepe> (enable) set logging server enable System logging messages will be sent to the configured syslog servers.    

pepe> (enable) sh logging  

Logging buffer size:          500         
timestamp option:     enabled 
Logging history size:         1 
Logging console:              enabled 
Logging server:               enabled {10.7.128.51}         
server facility:      LOCAL3         
server severity:      notifications(5) 
Current Logging Session:      enabled   

Facility            Default 
Severity         Current Session Severity 
-------------       -----------------------  ------------------------ 
cdp                 4                        4 
drip                2                        2 
dtp                 5                        5 
dvlan               2                        2 
earl                2                        2 
fddi                2                        2 
filesys             2                        2 
gvrp                2                        2 
ip                  2                        2 
kernel              2                        2 
mcast               2                        2 
mgmt                5                        5 
mls                 5                        5 
pagp                5                        5 
protfilt            2                        2 
pruning             2                        2 
radius              2                        2 
security            2                        2 
snmp                2                        2 
spantree            2                        2 
sys                 5                        5 
tac                 2                        2 
tcp                 2                        2 
telnet              2                        2 
tftp                2                        2 
udld                4                        4 
vmps                2                        2 
vtp                 2                        2   

0(emergencies)        
1(alerts)             
2(critical) 
3(errors)             
4(warnings)           
5(notifications) 
6(information)        
7(debugging) 	 	
```