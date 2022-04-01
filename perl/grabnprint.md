# grabnPrint

## Overview: 
From one sub take a file and stick it into an array.  Then output the ref of the array out of the sub.  Then in another sub, take that ref'ed array, defref it and do stuff (print) to it.  

## Code: 
```
#!/usr/bin/perl
#
# grab a file, convert to array, and do stuff

while ( my $arg = shift (@ARGV) ) {
    if ($arg =~ /-h/o) { usage(); exit; }
    if ($arg =~ /-d/o) { $debug=1; next; }
    if ($arg =~ /-f/o) { $inFile = @ARGV[0]; next; }
}

sub main {
    # main proc
    #
    my $dump = getFile($inFile);
    printArray($dump);
}
sub getFile {
    # get a file, put it into an array
    my $file = shift;
    my @array;
    open (FILE, "$file") or die "cant work with or open file [$file]\n ";
    while (my $line = <FILE>) {
        chomp($line); 
        push (@array,$line);
        print "(getfile) > $line \n", if $debug;
    }
    close FILE; 
    return(\@array);
}
sub printArray {
    # print out or do stuff to the array
    my $fileRef = shift;
    for my $line (@$fileRef){
        print "$line \n";
    }
}

sub usage {
    print "grab a file, convert to array, and print  \n";
    print "  \n";
    print "Usage: $0 [arguments] -f {filename}} \n";
    print "  \n";
    print "Arguments:  \n";
    print "  -h  Help.  This message\n";
    print "  -f {filename}  Enter the name of the file to grab  \n";
}
main();
```
## Output: 
```
./grabnPrint.pl -f text.txt
```

Output is what ever was the contents of the file "text.txt". 

