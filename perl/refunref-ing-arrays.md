# ref/unref-ing Arrays

Code:
```
#!/usr/bin/perl
#
#

use strict;

my @array = ("Fred","Joe", "Margaret", "Hilda");
my $refarray = \@array;

print "array = @array\n";
print "refarray = $refarray\n";
print "unrefed-array = @$refarray\n";
```

Output:
```
./tmp.pl
array = Fred Joe Margaret Hilda
refarray = ARRAY(0x8a435b8)
unrefed-array = Fred Joe Margaret Hilda
```


