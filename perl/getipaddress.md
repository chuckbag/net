# getipaddress

```
#!/usr/bin/perl -w
#
my $usr = "everyDNS_user";
my $pass = "everyDNS_password";
my $domain = "www.chuck.com";
my $eDNS = "/home/everydns/bin/eDNS.pl";

my $website = "api.hostip.info/get_html.php?ip";
my $searchcmd = "w3m $website";

my $debug = 0;

sub main {
    my $ip = getIP($searchcmd);
    runEDNS($ip, $usr, $pass, $domain, $eDNS);
}

sub getIP {
    my $ip;
    my $cmd = shift;

    print ">$cmd\n", if $debug;

    open(CMD, "$cmd |") || die $!;
    while (my $line = <CMD>) {

        #
        print  ">>>$line", if $debug;
        if ($line =~ /IP:\s+(\d+.\d+.\d+.\d+)/) {
                $ip = $1;
        }
    }
    print "IP = [$ip]\n", if $debug;
    close CMD;
    return $ip;
}

sub runEDNS {
    my $ip = shift;
    my $user = shift;
    my $pword = shift;
    my $site = shift;
    my $app = shift;

    my $cmd = "$app -u $user -p $pword -d $site -i $ip";

    print ">$cmd\n", if $debug;

    open(CMD, "$cmd |") || die $!;
    while (my $line = <CMD>) {
        #
        print  ">>>$line", if $debug;
    }
}

main();
__END__
```