# readGET

## The Code:
```
#!C:/perl/bin
use CGI qw(:standard);

print "Content-type: text/plain\n\n";
print "mod_perl 2.0 rocks dude!\n\n";
print "append the following string to the uri\n";
print "?username=your_name\n";

my $username = param('username') || "you didnt enter the username var";
print "your username is [$username]\n";
```

## Without the URI:
### Going to this URL:
- http://localhost/mod-perl/test.pl

### Provides the following output:
```
mod_perl 2.0 rocks dude!

append the following string to the uri
?username=your_name
your username is [you didnt enter the username var]
```

## With the URI:
### Going to this URL:
- http://localhost/mod-perl/test.pl?username=chuck

### Provides the following output:
```
mod_perl 2.0 rocks dude!

append the following string to the uri
?username=your_name
your username is [chuck]
```
