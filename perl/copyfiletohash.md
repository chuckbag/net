# copyFileToHash

Code:
```
 1 #!/usr/bin/perl 
 2 #
 3 #
 4 # enter a comma delimited text file as an argument to running this script,
 5 # and it will return all the cells of that text file seperated by colons.
 6 #
 7 
 8 use strict;
 9 
10 
11 
12 sub main {
13 	open (TEXTFILE, $ARGV[0]) or die "can't work with file $ARGV[0] \n";
14 	my $hashRef = makehash();
15 	echoHash($hashRef);
16 	close (TEXTFILE);
17 }
18 sub makehash {
19 	my $i = 0;
20 	my %file;
21 	print "---the values before they go into the hash table are---\n";
22 	while (<TEXTFILE>) {
23 		my ($k1, $k2, $v1) = split(/,/,$_); #split each TEXTFILE line via commas
24 		my $k = "$k1$k2";
25 		chomp($v1);
26 		$file{$k} = $v1;
27 		print "[$k] = [$k1]+[$k2], [$v1]\n";
28 	}
29 	return(\%file);
30 }
31 sub echoHash {
32 	my $hashRef = shift;
33 	print "---the output of the hash is---\n";
34 	while ( (my $key, my $value ) = each %{ $hashRef }) {
35 		print "$key \t $value\n";
36 	}
37 
38 }						
39 
40 
41 
42 main();
43 
44 __END__
```
Output:
```
$ cat test.csv
item1.1, item1.2, item1.3
item2.1, item2.2, item2.3
item with space3.1,  item with space3.2,  item with space 3.3
$
$ ./copyFileToHash test.csv
---the values before they go into the hash table are---
[item1.1 item1.2] = [item1.1]+[ item1.2], [ item1.3]
[item2.1 item2.2] = [item2.1]+[ item2.2], [ item2.3]
[item with space3.1  item with space3.2] = [item with space3.1]+[  item with space3.2], [  item with space 3.3]
---the output of the hash is---
item2.1 item2.2           item2.3
item1.1 item1.2           item1.3
item with space3.1  item with space3.2     item with space 3.3
$
```