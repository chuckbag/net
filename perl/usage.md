# usage

## Overview: 
See http://xkcd.com/1692/


## Usage Script:
```
#!/usr/bin/perl
#


use strict;
my $debug;

while (my $arg = shift (@ARGV))  {
    if ($arg =~ /-h/o ) {usage(); exit; }
    if ($arg =~ /-d/o ) {$debug =1; next; }
        next;
    }
print" hello world! \n";
print" hello debuged world!\n" if $debug;

sub usage {
    # explain how to use this
   print "this script shows how helpful a usage statement is \n";
   print "and how you can feed in a debug variable. \n";
   print " \n";
   print " \n";
   print "Usage: $0 [arguments] \n";
   print " \n";
   print "Arguments: \n";
   print "    -h  Help.  This message. \n";
   print "    -d  Debug. Include debug messages\n";
   print " \n";
}
```
## Output of Script:
```
$ perl usage.pl
 hello world!

$ perl usage.pl -h
this script shows how helpful a usage statement is
and how you can feed in a debug variable.


Usage: usage.pl [arguments]

Arguments:
    -h  Help.  This message.
    -d  Debug. Include debug messages


$ perl usage.pl -d
 hello world!
 hello debuged world!
```