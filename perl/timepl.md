# time.pl

Overview:
This script uses the localtime function to collect different date or time variables.

Code:
```
#!/usr/bin/perl -w
#

use strict;

sub main {
    my ($sec,$min,$hour,$mday,$mon,$year,$wday,$yday,$isdst) = localtime;

    my $cyear = ($year + 1900);
    my $monT = ($mon + 1);
    my $mon2 = sprintf "%02d", $monT;
    my $mday2 = sprintf "%02d", $mday;

    print " with the command:\n";
    print '[($sec,$min,$hour,$mday,$mon,$year,$wday,$yday,$isdst) = localtime]',"\n";
    print "we get:\n\n";
    print "\$sec: [$sec]\n";
    print "\$min: [$min]\n";
    print "\$hour: [$hour]\n";
    print " which makes the time: [$hour\:$min.$sec]\n";
    print "\$mday: [$mday] or [$mday2]\n";
    print "\$mon: [$mon] (01 + [$mon] = [$mon2])\n";
    print "\$year: [$year] (1900 + [$year] = [$cyear])\n";
    print " and makes the date: [$cyear$mon2$mday2]\n";
    print "\$wday: [$wday]\n";
    print "\$yday: [$yday]\n";
    print "\$isdst:[$isdst]\n";
}

main();

__END__
```
Example Output:
```
-bash-2.05b$ ./time
 with the command:
[($sec,$min,$hour,$mday,$mon,$year,$wday,$yday,$isdst) = localtime]
we get:

$sec:  [4]
$min:  [22]
$hour: [14]
 which makes the time: [14:22.4]
$mday: [7] or [07]
$mon:  [7] (01 + [7] = [08])
$year: [109] (1900 + [109] = [2009])
 and makes the date: [20090807]
$wday: [5]
$yday: [218]
$isdst:[1]
-bash-2.05b$
```
