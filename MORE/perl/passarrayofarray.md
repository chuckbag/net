# passArrayOfArray


Code:
```
 1 #!/usr/bin/perl 
 2 #
 3 #
 4 #
 5 
 6 use strict;
 7 
 8 sub main {
 9 	print "\n";
10 	print "---printing output of makeAofA---\n";
11 	my @arrayPointer = makeAofA();
12 	print "\n";
13 	
14 	print "--now passing the array between sub's---\n";
15 	print "the value of the array is:\n @arrayPointer \n\n";
16 	
17 	print "---printing output of echoAofA---\n";
18 	echoAofA(@arrayPointer);
19 	print "\n";
20 }
21 sub makeAofA {
22 	my @file;
23 	
24 	for (my $i = 0; $i < 5; $i++ ) {
25 		
26 		my $a = "a$i";
27 		my $b = "b$i";
28 		my $c = "c$i";
29 
30 		my @line = ($a, $b, $c);
31 		print @line, "\n";
32 		
33 		push (@file, [ @line ]);
34 	}
35 	return(@file);
36 }
37 sub echoAofA {
38 	my @file = @_;
39 
40 	for my $lineRef (@file) {
41 		print join(" : ", @$lineRef), "\n";
42 	}
43 }
44 
45 main();
46 
47 __END__
```
Command Output:
```
$ ./passArrayOfArray

---printing output of makeAofA---
a0b0c0
a1b1c1
a2b2c2
a3b3c3
a4b4c4

--now passing the array between sub's---
the value of the array is:
 ARRAY(0x876ae48) ARRAY(0x876ade8) ARRAY(0x876ad40) ARRAY(0x876ac20) ARRAY(0x8781bd4)

---printing output of echoAofA---
a0 : b0 : c0
a1 : b1 : c1
a2 : b2 : c2
a3 : b3 : c3
a4 : b4 : c4

$
```