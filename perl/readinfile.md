# readInFile

```
#!/usr/bin/perl
#
#
#

use strict;

my $filelocation = $ARGV[0];

sub main {
    my @file = getFile($filelocation);
    print "@file\n";
}

sub getFile {
    my $fileToOpen = shift;
    my @file;
    open (FILE, "$fileToOpen") or die "cant work with/open file [$fileToOpen]\n";
    while (my $line = <FILE>) {
        #chomp($line);
        next if ( $line =~ /^\s*$/ );   # ignore whitelines
        next if ( $line =~ /^\s*#/ );   # ignore comments
        push (@file, $line); #stick veriable line into file
        print "\t\t$line\n";
    }
    return(@file);
}

main();

__END__
```
