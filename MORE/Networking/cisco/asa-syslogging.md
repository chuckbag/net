# Syslogging

## Overview:
Firewalls can create an enormous about of logging data depending on what you want to track and what you want to keep.  Rather then requiring large disks on the firewalls to store all of those logs, you can send the log traffic off the firewall, and store it on a remote server.  This is also helpful in the event that the firewalls fail, you will likely be sent some amount of data to the remote server to help troubleshoot what went wrong even if the firewall is down. 

All logging can be done with the syslog protocol, and can be sent two ways, with TCP or UDP protocols.  Choosing the right method is critical, because the behavior of the firewall relates to how you send logging data. 

If you send logs over TCP, the firewall will assume that you **REQUIRE** that all log data is collected, AND IT WILL SHUT DOWN TRAFFIC THROUGH THE FIREWALL IF IT CANNOT CONNECT TO THE LOG SERVER!   With this method, you are guaranteed to log all transmissions, but if your connection to the logging server goes down, or if the logging server overloads/fails/crashes, your firewall will block traffic. 

Note that you can also define what port to send your syslog data over.  The default is udp:514, but it can be set to anything. 

## Configuring Local Logging:
The firewalls can log both remotely and locally.  Local logging is limited by storage space, so it is only useful if the logging level is low, but to enable it, you would do the following.

```
logging enable
logging buffer-size 100000
logging monitor informational
logging buffered informational
logging trap notifications
```

## Configuring Remote Logging:
You can have logging data sent to multiple servers, over any desired port.  If you specify transport over the TCP protocol, by default the firewall will block all traffic if it cannot connect to at least one server.  This "feature" can be disabled with the `logg permit-hostdown` command. 

In the following example, we are sending logs to two servers over port TCP:5149.  If the servers do not respond, the firewall will continue because of the `permit-hostdown` line.  The first line is in the format `logging host {outbound-int} {dest-ip} {prot}/{port}`

```
logging host oob-srv1 10.50.32.65 tcp/5149
logging host oob-srv1 10.50.32.66 tcp/5149
logging permit-hostdown
logging trap notifications
logging enable
```

## Debugging Logging Configuring:
To confirm that logging data is being sent properly, you can test with the following methods. 

To confirm that logging is setup and connecting to the syslog server, use the `show logging` command.  Note in this example, we see that the firewall cannot connect to the logging server.

```
# sh logging
Syslog logging: enabled
    Facility: 20
    Timestamp logging: disabled
    Standby logging: disabled
    Debug-trace logging: disabled
    Console logging: disabled
    Monitor logging: level informational, 607390808 messages logged
    Buffer logging: level informational, 607390808 messages logged
    Trap logging: level notifications, facility 20, 19348242 messages logged
        Logging to oob-srv1 10.50.32.65 tcp/5149 Not connected since Sat, 12 May 2012 01:02:43 UTC
        Logging to oob-srv1 10.50.32.66 tcp/5149 Not connected since Sat, 12 May 2012 01:02:43 UTC
    Permit-hostdown logging: enabled
    History logging: disabled
    Device ID: disabled
    Mail logging: disabled
    ASDM logging: disabled
%ASA-6-302015: Built outbound UDP connection 294447380 for i1-srv2:10.50.81.33/7651 (10.50.81.33/7651) to n1-srv1:10.50.144.33/34935 (10.50.144.33/34935)
%ASA-6-302015: Built outbound UDP connection 294447381 for i1-srv2:10.50.81.32/7651 (10.50.81.32/7651) to n1-srv1:10.50.144.33/36782 (10.50.144.33/36782)
```

## References: