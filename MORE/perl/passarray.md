# passArray

Code:
```
 1 #!/usr/bin/perl 
 2 #
 3 #
 4 #
 5 
 6 use strict;
 7 
 8 
 9 sub main {
10 	my @outArray = makeArray();
11 	#print @outArray, "\n";
12 	echoArray(@outArray);
13 }
14 sub makeArray {
15 	my @array = ("one", "two", "three");
16 	#print @array, "\n";
17 	return (@array);
18 }
19 sub echoArray {
20 	print @_, "\n";
21 	print join(", ", @_), "\n";
22 }
23 main();
24 
25 __END__
```
Script Running:
```
$ ./passArray
onetwothree
one, two, three
$
```
