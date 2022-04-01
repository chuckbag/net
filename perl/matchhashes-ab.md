# matchHashes-a+b

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
20 # the value in %comp writes over any value that might 
21 # have been in %people
22 my %comp_plus_people = (%people, %comp);
23 
24 
25 # print hash function:
26 while ( (my $key, my $value ) = each %comp_plus_people) {
27 	print "$key \t $value\n";
28 }
```
Output Example:
```
$ ./matchHashes-a+b
betsy
joe      cool
charlie  crap
zander   crap
fred
$
```