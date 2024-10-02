# transposeArray

The Code:
```
#!/usr/bin/perl

my @rows = ();
my @transposed = ();

# This is each row in your table
push(@rows, [qw(0 1 2 3 4 5 6 7 8 9 10)]);
push(@rows, [qw(6 7 3 6 9 3 1 5 2 4 6)]);

for my $row (@rows) {
  for my $column (0 .. $#{$row}) {
    push(@{$transposed[$column]}, $row->[$column]);
  }
}

for my $new_row (@transposed) {
  for my $new_col (@{$new_row}) {
      print $new_col, " ";
  }
  print "\n";
}
```
The Results:
```
0 6
1 7
2 3
3 6
4 9
5 3
6 1
7 5
8 2
9 4
10 6
```

