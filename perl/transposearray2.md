# transposeArray2

## The Issue:
The goal to over come with this script is to take the data from a spreadsheet, and parse it into multiple files.  The spreadsheet would have each object (like a car) in columns, with the first column listing the variables to define.  For example:
 
 |  |  |  |
 |--|--|--|
 name | bug	 | truck
 vendor	| vw	| ford
 model	| super beetle | t150
 seats	| 4	| 2

The output should separate each object into files, that includes the variable names.  For example: 

File1:

 |  |  |
 |--|--|
 |name | bug|
 vendor |	 vw
 model	 | super beetle
 seats	| 4

File2:

 |  |  |
 |--|--|
 name |  truck
 vendor	| ford
 model	 | t150
 seats	| 2

## The Code:
```
#!/usr/bin/perl -w
#
use strict; 

# start with a list of three objects 1, 2, and 3
# each has 5 variables a-e.  Each object is listed in 
# columns, and each row defines another variable of that object.
#
# Take this one input file, and create multiple output files, 
# one per object.

# grab rows in a sheet: 
my @row1 = ("key-a", "1a", "2a", "3a"); 
my @row2 = ("key-b", "1b", "2b", "3b"); 
my @row3 = ("key-c", "1c", "2c", "3c"); 
my @row4 = ("key-d", "1d", "2d", "3d"); 
my @row5 = ("key-e", "1e", "2e", "3e"); 

# push them in a array
my @sheet = ( 
    \@row1, 
    \@row2, 
    \@row3, 
    \@row4, 
    \@row5 
); 

# show the original sheet:  
print "Input files are: \n"; 
for my $rowRef1 (@sheet) {
    for my $cell1 (@{$rowRef1}) {
        print "cell1 = $cell1 ";
    }
    print "\n";
}
print "\n";

# transpose the rows 
my @tSheet = ();

for my $rowRef2 (@sheet) {
    for my $cell2 ( 0 .. $#{$rowRef2} ) {
        push ( @{$tSheet[ $cell2 ]}, $rowRef2 ->[ $cell2 ] ); 
    }
}

# show the outputted multiple sheets: 
print "Output files are: \n";
for my $rowRef3 (@tSheet) {
    my $i=0;
    for my $cell3 (@{$rowRef3}) {
        print "@{ $tSheet[0] }[$i] :"; 
        print "cell3 = $cell3 \n";
        $i++;
    }
    print "\n";
}
```
## The Results:
```
Input files are:
 key-a  1a  2a  3a
 key-b  1b  2b  3b
 key-c  1c  2c  3c
 key-d  1d  2d  3d
 key-e  1e  2e  3e

Output files are:
key-a : key-a
key-b : key-b
key-c : key-c
key-d : key-d
key-e : key-e

key-a : 1a
key-b : 1b
key-c : 1c
key-d : 1d
key-e : 1e

key-a : 2a
key-b : 2b
key-c : 2c
key-d : 2d
key-e : 2e

key-a : 3a
key-b : 3b
key-c : 3c
key-d : 3d
key-e : 3e
```
