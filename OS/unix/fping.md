# fping

## Overview
fping is a utility that allows you to ping multiple sites with one command. The following doc reviews some of it's capabilities, and gives examples.

## Cookbook
To ping an entire network
```bash
fping -g 192.168.1.0/24
```
or
```bash
fping -g 192.168.1.0 192.168.1.255
```

To ping an entire network and only see the links that are alive.
```bash
fping -g 192.168.1.0/24 2>/dev/null | grep -v unreachable
```
This version includes the average ping response time 
```bash
fping -e -g 192.168.1.0/24 2>/dev/null | grep -v unreachable
```
The following perl script will check a list of hosts and send mail if any are unreachable. It uses the open2 function which allows a program to be opened for reading and writing. fping does not start pinging the list of systems until it reads EOF, which it gets after INPUT is closed. Sure the open2 usage is not needed in this example, but it's a good open2 example none the less.
```bash
#!/usr/bin/perl
require 'open2.pl';

$MAILTO = "root";

$pid = &open2("OUTPUT","INPUT","/usr/local/bin/fping -u");

@check=("slapshot","foo","foobar");

foreach(@check) {  print INPUT "$_\n"; }
close(INPUT);
@output=<OUTPUT>;

if ($#output != -1) {
 chop($date='date');
 open(MAIL,"|mail -s 'unreachable systems' $MAILTO");
 print MAIL "\nThe following systems are unreachable as of: $date\n\n";
 print MAIL @output;
 close MAIL;
}
```
Another good example is when you want to perform an action only on hosts that are currently reachable.
```bash
#!/usr/bin/perl

$hosts_to_backup = 'cat /etc/hosts.backup | fping -a';

foreach $host (split(/\n/,$hosts_to_backup)) {
  # do it
}
```