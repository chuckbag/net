# listDir2

This looks at a specified directory (that apache is allowed to look at), and prints out the contents.  In this case, apache is located in the dir: `C:\zangweb\Apache2`, and it's looking in the dir `C:\zangweb\Apache2\logs`  (because of the `./logs` dir request). 

The Code
```
#!C:/perl/bin
use strict;

use CGI ( );
use IO::Dir ( );

my $q = CGI->new;
print $q->header("text/plain");
my $dir = IO::Dir->new("./logs");
print join "\n", $dir->read;
```
The output
http://localhost/mod-perl/test.pl
```
.
..
access.log
error.log
httpd.pid
```