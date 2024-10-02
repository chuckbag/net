# syslog

## Facilities and Severity
The following notes are a rough overview of the basic layout of the syslog configuration. Note that this is not always the case with non-*nix systems. For example, Cisco equipment has their own severity listing.
There are two different things to note for syslog. Syslog has to keep track of who is sending the message, and it needs to figure out how important the message is. The WHO part of this, who the message came from, is called the Facility.

examples of facilities are:

| facility | Description | 
|--|--|
kern   | the kernel
mail   | them mail subsystem
lpr    | the printing subsystem
deamon | system server processes
auth   | the login authentication system

As far as the importance level is concerned, this is called the severity. In general, when you denote a level of severity, it also includes all levels above it. (note: you can filter for specific severities with the = flag.)

examples of severity, in order of decreasing seriousness

| severity | Description | 
|--|--|
emerg  | system panic
alert  | serious error (immediate attention)
crit   | critical errors like drive failures
err   |  errors
warn  |  warnings
notice | non-critical messages
info   | informative messages
debug  | extra info for tracking problems
none   | ignore messages form this facility
mark   | (not included with *) selects messages every 20 min

Generally speaking, if I want all messages from the mail program that are really big problems to be emailed to my pager, I would filer on mail.crit and send it's output to sendmail to my pager. Note that (mail = the facility).and (crit = is critical errors and above) are to goto my pager.

## Facilities for External Hosts
Syslogd can receive messages from outside hosts. These messages have their own facilities. Instead of a collection of different types of facilities, we have seven facilities labeled local0 to local6. The host must send all it's logging as local#, then the syslogd server will accept it and put it in the correct location as per the conf file.
