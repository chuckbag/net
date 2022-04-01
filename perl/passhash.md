# passHash

Code:
```
 1 #!/usr/bin/perl
 2 
 3 use strict;
 4 
 5 sub main {
 6 	my $hashRef;
 7 	
 8 	print "---print methodOne---\n";
 9 	$hashRef = methodOne();  showHash($hashRef); # run methodOne and print
10 	print "---print methodTwo---\n";
11 	$hashRef = methodTwo();  showHash($hashRef); # run methodTwo and print
12 }
13 
14 sub methodOne {
15 	my %hash = (
16 		key1	=> "value1",
17 		key2	=> "value2",
18 	);
19 	return \%hash;
20 }
21 
22 sub methodTwo {
23 	my $hashRef = {
24 		key1	=> "value1",
25 		key2	=> "value2",
26 	};
27 	return $hashRef;
28 }
29 
30 sub showHash {
31 	my $hashRef = shift;
32 
33 	foreach my $key (keys %$hashRef) {
34 		print "$key is $hashRef->{$key}\n"; # the same as:
35 		print "$key is $$hashRef{$key}\n";
36 	}
37 }
38 
39 main();
40 
41 __END__
```
Output:
```
$ ./passHash
---print methodOne---
key2 is value2
key2 is value2
key1 is value1
key1 is value1
---print methodTwo---
key2 is value2
key2 is value2
key1 is value1
key1 is value1
$
```