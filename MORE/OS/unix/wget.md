# wget

## Overview:
Wget is a great utility for taking a remote site and mirroring it to your site. It gives you tons of controls so that you only copy specific file types, specific depths on directory trees, and specific file sizes, to only name a few. The biggest thing to mention about wget, is its ability to handle connections that continuously disconnect.

At this point, this document is just another wget primer. All you need to know is located at the above link. I am adding this so that I have a standard setup for mirroring .mp3 servers.

## Some Flags to note:
don't download, only cat to screen:
```
wget -qO- https://10.50.82.32/status.html 
```

ignore the https certificate.  (and do the same as above)
```
wget -qO- https://10.50.64.20/status.html --no-check-certificate
```


## Example: 
### Grab mp3's from an ftp server
Here's an example to go to a ftp site, walk it up to two directories deep, and grab all the mp3 files
```
wget -nH --cut-dir=2 -m -np -A mp3,MP3 ftp://anonymous:chuck@10.10.10.10/pub/mp3
```

Looking at it's components you have the following: 
- `ftp://anonymous:chuck@10.10.10.10/pub/mp3` Means that it will try and log into the ftp server `10.10.10.10`, and into the directory `/pub/mp3`. It will try and log in to that server with the user name anonymous, and the password chuck.
- `-A mp3,MP3` Means that it will "Accept" only files that have the characters mp3 or MP3 in them.
- `-np` or `--no-parent` "Do not ever ascend to the parent directory when retrieving recursively. This is a useful option, since it guarantees that only the files below a certain hierarchy will be downloaded. See section Directory-Based Limits for more details."
- This means that it should not go above the directory that you defined. Thus, if we stated in the url, the directory `/pub/mp3`, then wget will not go up to the directory `/pub`. It will only stay within or below the `mp3` directory. `-m` or `--mirror` Turn on options suitable for mirroring. This option turns on recursion and time-stamping, sets infinite recursion depth and keeps FTP directory listings. It is currently equivalent to `-r -N -l inf -nr`.



## References: 
- [official wget webpage](http://www.gnu.org/software/wget/wget.html)
- [Man Page](http://www.gnu.org/manual/wget/html_mono/wget.html): wget manual