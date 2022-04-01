# RefOfArrays
Just and example of how you can work with pointers of arrays:

## The Code
```
!/usr/bin/perl -w
#
#
use strict;

sub main {
    #
    my $ref = makeArray();
    print "\$ref = $ref \n";
    print "\n";
    print "\@ref = @$ref \n";
}

sub makeArray {
    my @array = ("one", "two", "three");
    print @array, "\n";
    return ( \@array );
}
main();
__END__
```

## output:
```
onetwothree
$ref = ARRAY(0x1098ae0)

@ref = one two three
```

## Discussion:

"When you encounter a scalar like `$foo`, you should be thinking "the scalar value of foo." That is, there's a foo entry in the symbol table, and the `$` funny character is a way of looking at whatever scalar value might be inside. If what's inside is a reference, you can look inside that (dereferencing `$foo`) by prepending another funny character. Or looking at it the other way around, you can replace the literal foo in `$foo` with a scalar variable that points to the actual referent. This is true of any variable type, so not only is ` $$foo` the scalar value of whatever $foo refers to, but ` @$bar` is the array value of whatever `$bar` refers to, ` %$glarch ` is the hash value of whatever ` $glarch` refers to, and so on. " - [Programming Perl, Third Edition (8.3.1. Using a Variable as a Variable Name)](http://my.safaribooksonline.com/book/programming/perl/0596000278/references/pperl3-chp-8-sect-3)

