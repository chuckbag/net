# passHash2

Code:
```
#!/usr/bin/perl -w
#
use strict;

sub main {
	# define a hash ref
	my $hashRef = {};
	
	# start putting things into the hash
	makeHash($hashRef);
	
	# add to the hash
	for (my $i = 0; $i <5; $i++) {
		addToHash($i, $hashRef);
	}

	# print out the hash
	printHash($hashRef);
}
sub makeHash {
	my $hashRef = shift;
	$hashRef->{ 'key1' } = 'value1';
}

sub addToHash {
	my $i = shift;
	my $hashRef = shift;
	my $x = $i**$i;
	$hashRef->{$i} = $x;
}

sub printHash {
	my $hashRef = shift;
	foreach my $key (keys %$hashRef) {
		print "$key is $hashRef->{$key} \n";
	}
}
	
main ();
```
Output:
```
$ ./passHash2
4 is 256
1 is 1
3 is 27
0 is 1
key1 is value1
2 is 4
$
```