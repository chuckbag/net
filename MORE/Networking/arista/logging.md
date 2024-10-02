# Logging


setup logging to send to a remote syslog server 

```
logging buffered informational
logging host 10.153.10.25
logging on 
```

confirm its setup properly: 

```Shell
nee-swc1c01#sh logging
Syslog logging: enabled
    Buffer logging: level informational
    Console logging: level errors
    Monitor logging: level errors
    Synchronous logging: disabled
    Trap logging: level informational
        Logging to '10.153.10.25' port 514 in VRF default via udp
    Sequence numbers: disabled
    Syslog facility: local4
    Hostname format: Hostname only
    Repeat logging interval: disabled
```
