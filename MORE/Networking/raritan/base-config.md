# base config

## Setup Network via Console: 
Default login 
- `admin / raritan`

## Configure Network: 
```
admin > configuration
admin > Config > network
> interface enable true if lan1 dhcp false ip 10.33.32.10 mask 255.255.255.0 gw 10.33.32.1
Do you wish to commit these settings (no/yes) (default: no) yes
The system is now rebooting.
Your session will now be closed.
Username:
```

## Rebooting: 
rebooting command is under the maintenance section: 
```
admin > maintenance
admin > Maintenance > reboot
Rebooting the system will logoff all users.
Do you want to proceed with the reboot? (no/yes) (default: no) yes
The system is now rebooting.
```

