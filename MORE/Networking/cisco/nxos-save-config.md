# Save Config

Save the running config,

```
sw1p01# copy run start

[########################################] 100%
Copy complete, now saving to disk (please wait)...
Copy complete.
sw1p01#
```

Then cat the output so you can save it elsewhere.  

```
sw1p01# terminal length 0
sw1p01# sh run

!Command: show running-config
!Running configuration last done at: Tue Sep  1 20:46:32 2020
!Time: Tue Sep  1 20:52:39 2020
version 7.0(3)I7(8) Bios:version 4.5.0
[...]
```

