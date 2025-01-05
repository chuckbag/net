# Command Line

## clish vs. bash
Depending on how the user account is setup, when you login to a gateway (firewall) or SMS (database) you will be put either in bash (expert) or clish (checkpoint cli) mode.

The clish ("cli"-"shell") starts with a carrot "`>`", whereas, the bash prompt starts with a pound "`#`" and the prefix of "`Expert`".

### clish   

<img src="img/cmd01.png" width="400" alt="">
 
### bash

<img src="img/cmd02.png" width="500" alt="">

### bash password
before you can login to bash, you need to set its password (aka the enable password).  You do this with the "set expert-password" command.  You can also tab-complete commands in clish which will also show you possible alternative commands matching what you have already typed in.  

<img src="img/cmd03.png" width="500" alt="">

### Save config changes
any changes made to the firewall from the clish prompt need to be saved via the "`save config`" command

### Moving between clish and bash
The following commands will move you between the two input methods: 

clish -> bash
```
expert
```

bash -> clish
```
exit
```






## Bash Commands: 
### cpconfig
make changes to the licenses of the firewall, including changing the SIC password (#5 Secure Internal Communications). 

<img src="img/cmd04.png" width="500" alt="">

### tcpdump
capture data from the firewall interfaces.  To break out of the capture, press [Ctrl]+[c]

<img src="img/cmd05.png" width="300" alt="">

### shutdown
shutdown the firewall

### reboot
reboot the firewall

### fw stat
name of the security policy installed on the gateway

<img src="img/cmd06.png" width="600" alt="">

### fw unloadlocal
unloads the policy from the firewall.  In other words it removes the firewall rules pushed from the DB to the unit.  It converts the firewall to a more "virgin-like" state, but keeps routes and interface settings.  Good if something horrible was pushed, and you just need to get control back to the unit.  

<img src="img/cmd07.png" width="600" alt="">

note, that if you enter fw stat after you have unloaded the gateway, it will show without a running policy

<img src="img/cmd08.png" width="600" alt="">

### fw ver
view the running OS version on the firewall

<img src="img/cmd09.png" width="400" alt="">

### fw getifs
view the interfaces on the gateway

<img src="img/cmd10.png" width="500" alt="">

### netstat -rn
views the routing table

<img src="img/cmd11.png" width="500" alt="">

### netstat -an
view running services and the ports

### cpstat os -f cpu
stats on the firewalls cpu

<img src="img/cmd12.png" width="350" alt="">

### cpstat os -f multi_cpu
View the status of the different processors

<img src="img/cmd13.png" width="600" alt="">

### cpview
view the cpview utility to see ~lots~ of different stats on the firewall via a command prompt.

<img src="img/cmd14.png" width="300" alt="">

You can scroll up and down (1) to see more of the results.  You can also see multiple tabs (2) by pushing the left and right buttons

<img src="img/cmd15.png" width="600" alt="">

to get out of cpview, press [Ctrl]+[c]






## clish Commands: 
note that "`netstat`", "`cpstat`", and the "`fw`" commands work both in bash and clish.  

### show interfaces 
view all of the interfaces configured on the firewall

<img src="img/cmd16.png" width="300" alt="">

### show interface eth0
see the stats of one interface

<img src="img/cmd17.png" width="600" alt="">

### show route
view the routes defined on the gateway

<img src="img/cmd18.png" width="300" alt="">

### show users
view current user accounts allowed on the gateway

<img src="img/cmd19.png" width="600" alt="">

### Add user: 
To add a user, use the add user command, define the uid, and the home directory
```
add user sam uid 200 homedir /home/sam
```

<img src="img/cmd20.png" width="600" alt="">

set the password for the new user
```
set user sam newpass vpn123
```

<img src="img/cmd21.png" width="600" alt="">

set the roles for the new user with the Role Based Access subcommand
```
add rba user sam roles adminRole
```

<img src="img/cmd22.png" width="600" alt="">

confirm user with the `show users` command again: 

<img src="img/cmd23.png" width="300" alt="">

remove a user with the `delete user` command: 
```
delete user sam
```

### clear screen 
to clear your screen in checkpoint press [Ctrl]+[l]

### backup and restore 
first save the running config
```
save config
```

then make a backup of the local host
```
add backup local
```

<img src="img/cmd24.png" width="400" alt="">

view the status of the backup (is it still copying?)  
```
show backup status
```

<img src="img/cmd25.png" width="600" alt="">

view the backup file in expert mode.  Since it's stored in linux, you can scp it off as needed.  you can rename this file as needed to remind you of the status point

<img src="img/cmd26.png" width="200" alt="">

importing the backup is done with the 
```
set backup restore restore local <tab>
```

<img src="img/cmd27.png" width="600" alt="">


