# uriVars

## The Code
```
#!C:/perl/bin
use CGI qw(:standard);

print "Content-type: text/plain\n\n";
print "in the uri, you can append variables and values that this script can accept\n";
print "you start with a ?, list a varname=value and append additional ones with an &\n";
print "\n";
print "for example, you can append the following\n";
print "?var1=value1&var2=value2&var3=value3\n\n";

print "you entered the following:\n";
for my $key ( param() ) {
    print "[$key] = ", param($key), "\n";
```

## The output without any added variables
http://localhost/mod-perl/test.pl

```
in the uri, you can append variables and values that this script can accept
you start with a ?, list a varname=value and append additional ones with an &
 
for example, you can append the following
?var1=value1&var2=value2&var3=value3
 
you entered the following:
```

## The output with multiple added variables
http://localhost/mod-perl/test.pl?var1=value1&var2=value2&var3=value3

```
in the uri, you can append variables and values that this script can accept
you start with a ?, list a varname=value and append additional ones with an &

for example, you can append the following
?var1=value1&var2=value2&var3=value3

you entered the following:
[var1] = value1
[var2] = value2
[var3] = value3
```

