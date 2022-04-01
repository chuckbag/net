# ping_sweeper


Main Program File:
```
#!/usr/bin/perl -w
#
use strict;
my $pingList = $ARGV[0];

sub main {
    my $i = 0; # counting string
    my $maxping = 20; # max number of pings per host

    print "This program pings 10 packets to each of the device in the \n";
    print "file ping_sweeper.host then measures the packet loss and round trip time\n";
    print "file format is either <hostname> <tab> <ip> or one of the two.\n";
    print "\n";
    print "Router\t\t\t\t% Pkt. Loss\t\tRTT\n";

    open (FILE, "$pingList") or die "cant work with/open file [$pingList]\n";
    while (my $entry = <FILE>) {
        if ($i < $maxping) {
            #print "$i\n;
            #print "[$entry]\n";
            pingStatus($entry);
        }
        $i++;
    }
}


sub pingStatus {
    my $entry = shift;   # IP address to ping
    my @contact_device;  # ping output results
    my $name;            # hostname of device
    my $Router;          # IP address of device to ping

    ($name, $Router) = split(/\t/, $entry);
    if (! $Router) {
        chomp $name;
        $Router=$name;
    }
    #print "[$name/$Router]\n";
    #@contact_device = `/bin/ping -f -c 10 -a -i 0 -s 500 -l 20 $Router 2>&1`;
    @contact_device = `/bin/ping     -c 10 -a      -s 500 -l 3 $Router 2>&1`;
    foreach my $rawinformation (@contact_device) {
        #print "$rawinformation\n";
        if ( $rawinformation =~ /(\d{1,5}) packets transmitted, (\d{0,5}) received, (\d{0,5})% packet loss, time (\w+)/){
            printf "%-31s %-23d %-12s\n", $name, $3, $4;
            #print "$Router\t\t\t$3% pkt. loss\t\t$4\n";
        }
    }
}


main();
```


and the ping_sweeper.hosts file looks like this:
```
RT02    10.31.1.2
RT04    10.31.1.1
RT06    10.31.1.8
RT09    10.31.1.9
```
