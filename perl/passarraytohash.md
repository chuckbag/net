# passArraytoHash

Code:
```
#!/usr/bin/perl -w
use strict;

# 1a) define array
my @neighbors;
# 1b) define list of arrays
my %machines;

# 2a) populate array
@neighbors = ('a', 'b', 'c');
# 2b) stick it into the hash
$machines{'foo.ariba.com'} = [@neighbors];

# 3a) clear the value of the array (and then do something else)
@neighbors = ();

# 4a) repopulate the array
@neighbors = ('1', '2', '2');
# 4b) stick it into the hash
$machines{'bar.ariba.com'} = [@neighbors];

# 5) print out everything
for my $machine ( keys %machines )  {
        print $machine, ": ", join(", ", @{$machines{$machine}}),"\n";
}
```
Output
```
$ ./passArraytoHash
bar.ariba.com: 1, 2, 2
foo.ariba.com: a, b, c
$
```