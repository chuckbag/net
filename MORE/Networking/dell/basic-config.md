# Basic Config
hostname 
```
hostname myf10
```

local username and enabling ssh
```
username admin password password_123 privilege 15
crypto key generate rsa
<!--- keysize 2048 ---->
ip ssh server enable
```

management interface, routes and vty 
```
interface ManagementEthernet 1/1
 ip address 10.153.10.52/24
 no shutdown
!
management route 0.0.0.0.0 0.0.0.0 10.15.10.1
!
line vty 0
 exec-timeout 30 0
```

banners
```
banner login ^C
**************************************************************
*                  AUTHORIZED ACCESS ONLY                    *
**************************************************************
*          This system is the property of cmed.         *
* Disconnect IMMEDIATELY if you are not an authorized user ! *
**************************************************************
^C
banner motd ^C
******************************************************************************
          This computer system is the property of cmed.
Only authorized users are entitled to connect and/or login to  this  computer
system. Authorized users are personnel whose duty it is to  support
this system and who have been expressly authorized by the system administrator
for this purpose. If you are not sure whether you are authorized,  then  you
should DISCONNECT IMMEDIATELY and seek advice from your line manager.
Unauthorized access and/or modification of data contained within  this system
is a criminal offense under the Computer Misuse Act 1990.
Please note that all access attempts, commands entered  and  keystrokes may be
logged for security purposes and, by continuing, you consent to this.
******************************************************************************
^C
!
```

Saving configs and then viewing: 
```
term len 0
wr mem
sh run 
```
