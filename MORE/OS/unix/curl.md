# curl

## Basic viewing from console: 

Just look at a page and pipe to the console: 
```bash
[~]# curl http://google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
[~]#
```

or:  
- `-s` for not showing the status

```bash
[~]# curl -s http://api.hostip.info | grep "<ip>"
   <ip>38.111.225.242</ip>
[~]#
```

Look at an encrypted page, and ignore if it is self signed (`-k`) <br>
- `-k` to ignore the private cert

```bash
curl -k https://10.50.64.23/status/
ok (02)
```

Grep on the output, (and you get nasty standard out spooge).  
- `-k` to ignore the private cert

```bash
curl -k https://10.50.64.23/status/ | grep ok
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     8    0     8    0     0     67      0 --:--:-- --:--:-- --:--:--     0
ok (02)
```

To just get the grepped output, send the standard out to devnull. 
- `-k` to ignore the private cert

```bash
curl -k https://10.50.64.23/status/ 2>/dev/null | grep ok
ok (02)
```

View the HEADDERS!!! 
- `-v` for the headers
- `-s` for not showing the status 

```bash
[~]# curl -vs google.com 2>&1
* About to connect() to google.com port 80 (#0)
*   Trying 173.194.204.100...
* Connected to google.com (173.194.204.100) port 80 (#0)
> GET / HTTP/1.1
> User-Agent: curl/7.29.0
> Host: google.com
> Accept: */*
>
< HTTP/1.1 301 Moved Permanently
< Location: http://www.google.com/
< Content-Type: text/html; charset=UTF-8
< Date: Wed, 06 Jun 2018 14:44:44 GMT
< Expires: Fri, 06 Jul 2018 14:44:44 GMT
< Cache-Control: public, max-age=2592000
< Server: gws
< Content-Length: 219
< X-XSS-Protection: 1; mode=block
< X-Frame-Options: SAMEORIGIN
<
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
* Connection #0 to host google.com left intact
[~]#
```

Following redirects, where google.com is a redirect

```bash
~ $ curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
~ $
```

but if you use the L flag, you can have curl follow it. 
- `-L` for following redirects

```bash
~ $ curl -L google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head><meta content="Search the world's information, including webpages, images, videos and more. Google has many special features to help you find exactly what you're looking for." name="description"><meta content="noodp" name="robots"><meta content="text/html; charset=UTF-8" http-equiv="Content-Type"><meta content="/images/branding/googleg
[...]
```

## Save page to a file: 
