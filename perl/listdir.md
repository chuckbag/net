# listDir

```
#!/usr/bin/perl -w
#
use strict;

my $dir = ".";

opendir(DIR, $dir) or die "can't open directory [$dir]\n $!\n";
    my $name;
    while ($name = readdir(DIR)) {
        if ($name =~ /^\./) {next;} # hide all files that start with .
        print "\t$name\n";
    }
closedir(DIR);
```