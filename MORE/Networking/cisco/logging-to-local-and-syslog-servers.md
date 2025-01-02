# Logging to local and Syslog servers

- [Logging to local and Syslog servers](#logging-to-local-and-syslog-servers)
  - [Overview:](#overview)
  - [Enable Logging:](#enable-logging)
  - [Controlling logging to terminals:](#controlling-logging-to-terminals)
  - [Viewing local logging:](#viewing-local-logging)
  - [Confirming logging:](#confirming-logging)
  - [References:](#references)
    - [Links:](#links)
    - [Overview of the Logging Levels](#overview-of-the-logging-levels)
    - [Overview of the logging facilities](#overview-of-the-logging-facilities)


## Overview:
The following changes will setup your switch to save a small amount of logs locally, and to ship out all the logs to a remote syslog server.

## Enable Logging:
First set the size (in bites) for how much logs should stored on the local switch.  The range is 4096(def)-2147483647, and is entered with the `logging buffered size {#}` command.  To send logging data to off site storage (external syslog boxes), use the `logging {host}` command.  The default logging transport it udp 514, but if you add the `transport` part to the logging command, you can specify the port and protocol.  To specify what interface the logging data goes out, use the `source-interface` command.

```
conf t
logging buffered size 256000
logging source-interface Vlan950
logging 10.50.32.65 transport tcp port 5150 
logging 10.50.32.66 transport tcp port 5150
```

We can control how much(what kind of) data is sent to logging.  Limit the type of data sent to a terminal, use the `logging monitor {level}` command.  To limit the logging sent to a syslog server, use the `logging trap {level}` command.   When logs are sent to a syslog server, they are identified as to "where" they come from via the `logging facility {facility}` command.  For more on syslog theory, see syslog notes, and as a reference for the logging levels and facility's, see below. 

```
logging monitor 1
logging trap 6
logging facility local6
```

As well as logging system information, you can also log config changes made to the device.  You would do this through the archive series of commands.  You can control the number of previous commands stored.  This is done through the logging size {lines} command, where the number is the number of commands stored, with the default being 100.

```
archive
 log config
  logging enable
  logging size 100
  exit
 exit
end
```

You can also add more logging for a specific PORT related to some specific things on the switch.  This includes switch port link state, spanning-tree events and status changes to spanning tree, as well as changes to trunk ports.

```
interface Port-channel1
 logging event link-status
 logging event spanning-tree
 logging event status
 logging event trunk-status
 end
```

## Controlling logging to terminals:
Logging to either the terminal or console can be helpful for you to see what is happening at the time, but can also be a hassle if there is too much getting in the way of the work you are doing.  Above we discussed how to control what is sent to the devices, here we review how to display or not to display the logs, on your session. 

To have logging spit directly to your terminal use the term monitor statement.  (note the above `logging monitor {level}` command as a way to limit how much spooge goes to your terminal .)

```
term monitor
```

If you are on the console, and being blinded by lots of logs popping up on the screen, you can disable this with the command:

```
no logging console
```

## Viewing local logging:
To view the buffered logs on the switch

```
sh logg
```

To view the config change history

```
sh archive log config
```

## Confirming logging:
Other then viewing the logs on the syslog server, or running a tcpdump on the syslog server, you can also confirm syslog is setup properly on the switch by running  the command sh logg.  Note that in this example, the first server is not up, but the second is, and logs are being sent to it. 

```
sw#sh logg

Syslog logging: enabled (0 messages dropped, 0 messages rate-limited, 0 flushes, 0 overruns, xml disabled, filtering disabled)
No Active Message Discriminator.
No Inactive Message Discriminator.
    Console logging: disabled
    Monitor logging: level alerts, 0 messages logged, xml disabled,
                     filtering disabled
    Buffer logging:  level debugging, 100265 messages logged, xml disabled,
                    filtering disabled
    Exception Logging: size (4096 bytes)
    Count and timestamp logging messages: disabled
    File logging: disabled
    Persistent logging: disabled
No active filter modules.
    Trap logging: level informational, 100265 message lines logged
        Logging to 10.50.32.65  (tcp port 5150, audit disabled,
              link down),
              0 message lines logged,
              0 message lines rate-limited,
              0 message lines dropped-by-MD,
              xml disabled, sequence number disabled
              filtering disabled
        Logging to 10.50.32.66  (tcp port 5150, audit disabled,
              link up),
              4527 message lines logged,
              0 message lines rate-limited,
              0 message lines dropped-by-MD,
              xml disabled, sequence number disabled
              filtering disabled
Log Buffer (256000 bytes):

*Apr 10 06:40:31.034: %LINK-3-UPDOWN: Interface GigabitEthernet0/41, changed state to down
```


## References:
### Links:
- [Configuring System Message Logging](http://www.cisco.com/en/US/docs/switches/lan/catalyst3750x_3560x/software/release/12.2_55_se/configuration/guide/swlog.html): (Catalyst 3750-X and 3560-X Switch Software Configuration Guide, Release 12.2(55)SE)

### Overview of the Logging Levels
How important the issue is

Level Keyword | Level | Description                      | Syslog Definition
--------------|-------|----------------------------------|------------------
emergencies   | 0	  | System unstable	                 | LOG_EMERG
alerts	      | 1	  | Immediate action needed	         | LOG_ALERT
critical	  | 2	  | Critical conditions	             | LOG_CRIT
errors	      | 3	  | Error conditions	             | LOG_ERR
warnings	  | 4	  | Warning conditions	             | LOG_WARNING
notifications | 5	  | Normal but significant condition | LOG_NOTICE
informational | 6	  | Informational messages only	     | LOG_INFO
debugging	  | 7	  | Debugging messages	             | LOG_DEBUG


### Overview of the logging facilities
The "who"

Facility Type Keyword | Description
----------------------|------------
auth	            | Authorization system
cron	            | Cron facility
daemon	            | System daemon
kern	            | Kernel
local0-7	        | Locally defined messages
lpr	                | Line printer system
mail	            | Mail system
news	            | USENET news
sys9-14	            | System use
syslog	            | System log
user	            | User process
uucp	            | UNIX-to-UNIX copy system


