# AutoHTML
## The Code:
```
#!C:/perl/bin
#

use strict; 

use CGI;            #load cgi routines

my $q = CGI->new;   #create the cgi object
print $q->header;   #create the http header
print $q->start_html('hello world'); #start the html

print "stuff that I think is above the h1 marker\n";

print $q->h1('h1 header');  #level 1 header

print "stuff in what I think is the body\n";

print $q->end_html;   # end html
```
## The Output
- http://localhost/mod-perl/test2.pl

<span style="border-collapse:separate;color:rgb(0,0,0);font-family:Times New Roman;font-style:normal;font-variant:normal;font-weight:normal;letter-spacing:normal;line-height:normal;text-align:-webkit-auto;text-indent:0px;text-transform:none;white-space:normal;word-spacing:0px;font-size:medium">
<div class="sites-codeblock sites-codesnippet-block">stuff that I think is above the h1 marker
<h1>h1 header</h1>
stuff in what I think is the body</div>
<br>
</span>


## The HTML Source of the Output:
```
<!DOCTYPE html
    PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
     "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html xmlns="http://www.w3.org/1999/xhtml" lang="en-US" xml:lang="en-US"> 
<head> 
<title>hello world</title> 
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" /> 
</head> 
<body> 
stuff that I think is above the h1 marker
<h1>h1 header</h1>stuff in what I think is the body
 
</body> 
</html>
```
## Ref:
- http://perldoc.perl.org/CGI.html
