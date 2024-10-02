# du and df

## du (Disk Usage)
shows a file or directory, and the file sizes 
(also see [wikipedia](http://en.wikipedia.org/wiki/Du_%28Unix%29))
```bash
# du -h
16K     ./lost+found
4.0K    ./cups
4.0K    ./samba/cores/smbd
4.0K    ./samba/cores/nmbd
12K     ./samba/cores
220K    ./samba
4.0K    ./news
4.0K    ./sandbox
32K     ./portage/elog
36K     ./portage
8.0K    ./mysql
4.0K    ./squeezecenter
4.0K    ./apache2
1020K   .
```

## df (Disk Free)
shows the different drives and their usage
(also see [wikipedia](http://en.wikipedia.org/wiki/Df_%28Unix%29))
```bash 
# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/hda2              74G  2.8G   67G   4% /
udev                   10M  188K  9.9M   2% /dev
/dev/hdb2             7.4G  7.4G     0 100% /var/log
/dev/hde1             233G  134G  100G  58% /disk1
/dev/hdf1             190G  164G   26G  87% /disk2
shm                    61M     0   61M   0% /dev/shm
```
