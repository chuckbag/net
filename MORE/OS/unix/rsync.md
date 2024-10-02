# rsync

## Overview: 
Basic copying of one file to different location on a different server via ssh.
```
rsync --progress -ave ssh {local-dir} {user@server:/remote-dir}
```

## flags to note:    

### Dry Run
If you want to see what files to transfer, but not actually transfer them, you can use the dry run flag: 

To see what files do not match, but to not transfer anything.
```
-n, --dry-run
```


Compare remote and local directories and recursively list all of the files that exist at the remote directory that are not at the local. (also ignore .svn directories)
```
rsync -avne ssh --exclude=*.svn {user@server}:/remote-dir {local-dir}| grep -v /$
```


View how much data will be transferred
```
rsync -an --stats sourcedir/ destdir/
```
and get a resulting output of : 
```
Number of files: 2
Number of files transferred: 1
Total file size: 4096 bytes
Total transferred file size: 2048 bytes
Literal data: 0 bytes
Matched data: 0 bytes
File list size: 82
File list generation time: 0.013 seconds
File list transfer time: 0.000 seconds
Total bytes sent: 110
Total bytes received: 32
```

## References:
- [RSync - Overview](http://tutorials.jenkov.com/rsync/overview.html), May 2014, Jakob Jenkov