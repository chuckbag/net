# String Hashes

Take two hashes, and string them together

Code: 
```
#!/usr/bin/perl
#
#

sub main {
    my ($h1,$h2) = makeHash();
    printHash($h1,$h2);
}

sub makeHash {
    # take these two hashs
    my %hash1= (
        "X1" => "A1",
        "X2" => "A2",
        "X3" => "A3",
        "X4" => "A4"
    );
    my %hash2 = (
        "A1" => "a1",
        "A2" => "a2",
        "A3" => "a3",
        "A4" => "a4"
    );

    # then line them up so we get an output like:
    # "X1" and "a1"
    print "printing straight up hashes\n";

    for my $X ( keys %hash1 ) {
        my $A = $hash1{$X};
        print "X = [$X], A = [$A] a = [$hash2{$A}] \n";

    }

    # but if we want to also test how to send these hashes to other
    # sub's, then we need to work with references.
    #
    return (\%hash1, \%hash2);
}

sub printHash {
    # same process for printing, but this time with referenced hashes
    my $H1 = shift;
    my $H2 = shift;

    print "\nprinting referenced hashes \n";
    for my $X ( keys %$H1 ) {
        my $A = $H1->{$X};
        print "X = [$X], A = [$A] a = [$H2->{$A}] \n";

    }
}
main();
__END__
```

Results: 
```
./stringHash.pl
printing straight up hashes
X = [X2], A = [A2] a = [a2]
X = [X3], A = [A3] a = [a3]
X = [X4], A = [A4] a = [a4]
X = [X1], A = [A1] a = [a1]

printing referenced hashes
X = [X2], A = [A2] a = [a2]
X = [X3], A = [A3] a = [a3]
X = [X4], A = [A4] a = [a4]
X = [X1], A = [A1] a = [a1]
```




