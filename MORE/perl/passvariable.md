# passVariable
Code is:
```
#!/usr/bin/perl
#
#

my $a = 3;     # define the number to a
my $b;
$$b = $a;   # assigin the stack id of a to be

print "\$a is [$a] \n";
print "\$b is the ref of a, which is [$b] \n";
print "\$\$b is [$$b] \n ";
```

Output is:
```
$a is [3]
$b is the ref of a, which is [SCALAR(0xa031170)]
$$b is [3]
```