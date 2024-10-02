# matchHashes-a_with_b_values

Code:
```
 1 #!/usr/bin/perl 
 2 #
 3 #
 4 #
 5 
 6 use strict;
 7 
 8 my %people = (
 9 	"charlie"	=>	"",
10 	"fred"		=>	"",
11 	"joe"		=>	"",
12 	"betsy"		=>	""
13 );
14 my %comp = (
15 	"charlie"	=>	"crap",
16 	"joe"		=>	"cool",
17 	"zander"	=>	"crap"
18 );
19 
20 # when comp has values for people, write over the people value.
21 while ( (my $key1, my $value1 ) = each %people) {
22 	if (defined ( $comp{$key1} ) ) {
23 		$people{$key1} = $comp{$key1}
24 	}
25 }
26 
27 # print hash function:
28 while ( (my $key, my $value ) = each %people) {
29 	print "$key \t $value\n";
30 }
```
Output:
```
$ ./matchHashes-a_with_b_values
betsy
joe       cool
charlie   crap
fred
$
```