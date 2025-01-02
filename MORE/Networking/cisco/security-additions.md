# Security Additions

## Turn off Default Services
These default services should be turned off.

```
no service udp-small-servers 
no service tcp-small-servers 
no service finger 
no ip source-route 
no ip redirects 
no ip unreachables 
no ip proxy-arp 
no cdp run
```

## Control telnet Access
Rather than having a single password to log into the router, setup individual login's for each user, and then a superuser password for root access. Also create an access list to prevent telnet access to the router.

```
user {username} password {users_password} 
line vty 0 4 	
  login local 	
  access-class 3 in 
  access-list 3 permit {address of allowable hosts to telnet}
```

## Crank down SNMP
SNMP is a HUGE gaping hole! Not having it on is best, but if you need it for management, then tighten it down with access lists and random community strings.

```
snmp-server community secret RO 1 
access-list 1 permit {address of snmp server}
```

