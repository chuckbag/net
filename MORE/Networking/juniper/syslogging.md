# syslogging

## Send traffic to a syslog server: 
this is for both system "any" and traffic "any" logs: 
```
set system syslog host 192.30.80.76 any any
```

## View syslog config: 
at edit prompt, 
```
show system syslog
```


## References: 
- [SRX Getting Started - Configure System Logging](http://kb.juniper.net/InfoCenter/index?page=content&id=KB16502): Jan 2014
- [Junos OS System Logging Facilities and Message Severity Levels](http://www.juniper.net/documentation/en_US/junos14.2/topics/reference/general/syslog-facilities-severity-levels.html): junos 14.2
