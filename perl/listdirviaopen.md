# listDirViaOpen

```
#!/usr/bin/perl -w
#
# run the shell command "$cmd" and work with it's output.
#
#

use strict;

sub main {
        my $cmd = "ls -lF";

        # method #1
        #my @array = `$cmd`;
        # foreach my $line (`$cmd`) {
        #      # do stuff..
        # }

        my @array;
        open(CMD, "$cmd |") || die $!;
        while (my $line = <CMD>) {
                # do stuff
                push(@array, $line);
        }

        close CMD;

        open(OUT, "> newfile.txt") || die $!;
        print OUT @array;
        close OUT;

}

main();
```

