# copyFileToAofA

Code:
```
 1 #!/usr/bin/perl 
 2 #
 3 #
 4 # enter a comma delimited text file as an argument to running this script,
 5 # and it will return all the cells of that text file separated by colons.
 6 #
 7 
 8 use strict;
 9 
10 sub main {
11 	open (TEXTFILE, $ARGV[0]) or die "can't work with file $ARGV[0] \n";
12 	my $arraypointerRef = makearray();
13 	echoAofA($arraypointerRef);
14 	close (TEXTFILE);
15 }
16 sub makearray {
17 	my $i = 0;
18 	my @file;
19 	while (<TEXTFILE>) {
20 		my @line = split(/,/,$_); #split each TEXTFILE line via commas
21 		push (@file, [ @line ]);
22 	}
23 	return(\@file);
24 }
25 sub echoAofA {
26 	my $fileRef = shift;
27 
28         for my $lineRef ( @{ $fileRef } ) {
29                 print join(" : ", @$lineRef), "\n";
30         }
31 }						
32 
33 
34 
35 main();
36 
37 __END__
```
Command Output:
```
$ cat test.csv
item1.1, item1.2, item1.3
item2.1, item2.2, item2.3
item with space3.1,  item with space3.2,  item with space 3.3
$
$ ./copyFileToAofA test.csv
item1.1 :  item1.2 :  item1.3

item2.1 :  item2.2 :  item2.3

item with space3.1 :   item with space3.2 :   item with space 3.3

$
```