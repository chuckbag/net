# config serial ports
You can configure each port via the GUI, but its painfully slow, so lets avoid that and make the changes at the CLI.

## Navigate to the port config section: 
to be able to modify ports, you need to get to the port section via the two commands, configuration and ports: 
```
admin > configuration
admin > Config > ports
admin > Config > Port >
```

## Name the port
To name the port, you specify the port, and what the name should be
```
admin > Config > Port > config port 1 name swa1p01
Port 1 : Configuration Saved
```

## Assign IP to the Port
To specify an IP you can use to directly ssh to the console port, use the dpaip command as such: 
```
admin > Config > Port > config port 1 dpaip 10.120.32.201
Port 1 : Configuration Saved
The system will need to be rebooted for DPA changes to take effect.
admin > Config > Port >
```

## Allow Multiple Users to Connect To Port at Same Time: 
You might want to allow multiple users to connect to a single port at the same time.  This has two benefits, one is that you can show someone else how to do something (with you and them on the same port at the same time), and if you loose your connection, you can simply reconnect, and not have to wait for a previous session to time out.  
```
admin > Config > Port > config port 1 multiwrite true
Port 1 : Configuration Saved
```

## Do all the Above at On Command: 
The above statements are nice if those are all you want to enter, but you can also enter them all on the same line at the same time.  this saves time when modifying many ports.  For each port just enter all the info together.  
```
admin > Config > Port > config port 1 name swa1p01 dpaip 10.120.32.201 multiwrite true
Port 1 : Configuration Saved
```

## Enable IP access to the ports: 
You need to enable dpa (per port direct port access) on the console server before anyone can connect directly to them.  To do this first go into the services section: 
```
admin >
admin > configuration
admin > Config > services
admin > Config > Services >
```

Then enable DPA:
```
admin > Config > Services > dpa mode IP
The system will need to be rebooted for DPA changes to take effect.
```

Then reboot to enable the changes:
```
admin > Config > Services > top
admin > maintenance
admin > Maintenance > reboot
Rebooting the system will logoff all users.
Do you want to proceed with the reboot? (no/yes) (default: no) yes
The system is now rebooting.
```

