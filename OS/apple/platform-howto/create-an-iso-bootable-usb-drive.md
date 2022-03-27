
# create an ISO bootable USB drive

## Confirm location of USB Stick

insert the USB stick in the mac and run diskutil to check it out: 
```
$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage Macintosh HD            250.1 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                            Home                   +249.8 GB   disk1
                                 Logical Volume on disk0s2
                                 3F16FAC9-AC72-4136-8563-8C053817D52D
                                 Unlocked Encrypted

/dev/disk2 (disk image):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     Apple_partition_scheme                        +19.8 MB    disk2
   1:        Apple_partition_map                         32.3 KB    disk2s1
   2:                  Apple_HFS Flash Player            19.7 MB    disk2s2

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.5 GB    disk3
   1:                 DOS_FAT_32 KINGSTON                15.5 GB    disk3s1
```
note the identifier of the mount (disk3s1)

## Clear the USB Drive
```
$ diskutil partitionDisk /dev/disk2 1 "Free Space" "unused" "100%"
```

## Convert the ISO file
Convert the original file (source_file.iso) to a .dmg file (you enter destination_file.img as the filename, and it will save destination_file.img.dmg)
```
$ hdiutil convert -format UDRW -o destination_file.img source_file.iso
```

## Create bootable USB drive with image
Burn the iso onto the stick with the dd command.  (note the "r" in front of the drive identifier).  Also note that in the step above you called the file "destination_file.img" but in this command you use "destination_file.img.dmg" which is how the previous hdiutil command will save the file. 
note the format of the command: `sudo dd if={path_of_iso} of=/dev/r(drive) bs=1m `

```
$ sudo dd if=destination_file.img.dmg of=/dev/disk2 bs=1m
680+0 records in
680+0 records out
713031680 bytes transferred in 41.008039 secs (17387607 bytes/sec)
```

note that the dd command does not output the status of the transfer, so you just have to keep an eye on it and hang out.  
Also, depending on the disk size, this could _TAKE A REALLY LONG TIME_!!!

## Eject the USB drive 
then eject the volume before removing: 
```
$ diskutil eject /dev/disk2 
Disk /dev/disk3s1 ejected
```

## References:
- [How to Copy an ISO to a USB Drive from Mac OS X with dd](https://osxdaily.com/2015/06/05/copy-iso-to-usb-drive-mac-os-x-command/): OSXDaily, Jun 5th, 2015
- [Create bootable USB stick from ISO in Mac OS X](https://blog.tinned-software.net/create-bootable-usb-stick-from-iso-in-mac-os-x/), Tinned-software, May, 2013


