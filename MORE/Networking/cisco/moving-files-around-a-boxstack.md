# Moving files around a box/stack
view the contents of a volume: 

```
Switch#dir flash2:
Directory of flash:/
    2  -rwx         616   Mar 1 1993 00:07:52 +00:00  vlan.dat
    3  -rwx    16470273   Mar 1 1993 01:36:07 +00:00  c3750-ipservicesk9-mz.122-58.SE2.bin
    5  -rwx        1956   Mar 1 1993 00:06:14 +00:00  config.text
    6  drwx         192   Mar 1 1993 00:12:27 +00:00  c3750-ipservicesk9-mz.122-50.SE
    7  -rwx        3096   Mar 1 1993 00:03:36 +00:00  multiple-fs
    8  -rwx        1918   Mar 1 1993 00:06:14 +00:00  private-config.text
32514048 bytes total (1026560 bytes free)
Switch#
```

delete the contents of a directory and everything within it: 

```
Switch#delete /force /recursive flash:/c3750-ipservicesk9-mz.122-50.SE/*
Switch#
```

or to delete a directory, and everything within it: 

```
Switch#delete /force /recursive flash:/c3750-ipservicesk9-mz.122-50.SE
Switch#
```
