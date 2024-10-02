
# create an ISO bootable USB drive

## Confirm location of USB Stick

insert the USB stick in the mac and run `diskutil` to check it out: 

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

In this example the `/dev/disk2` is the usb memory stick. 



## Clear the USB Drive

### Mac Boot Drive

To wipe the USB stick, run the `diskutil partitionDisk` command 

``` shell
~$ diskutil partitionDisk /dev/disk4 1 "Free Space" "unused" "100%"
Started partitioning on disk4
Unmounting disk
Creating the partition map
Waiting for partitions to activate
Finished partitioning on disk4
/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *15.6 GB    disk4
   1:                        EFI EFI                     209.7 MB   disk4s1
                    (free space)                         15.4 GB    -
~$
```
Which will keep the formatting (FAT) of the usb stick, but will do the following:  
- `1` = disk should get repartitioned with 1 partition 
- `"Free Space"` = do not change the format (see output of command `diskutil listFilesystems` to see list of formatting options)
- `"unused"` = name of the usb stick
- `"100%"` = format 100% of the disk 

### Windows Boot Drive

``` shell
~$ diskutil eraseDisk FAT32 "WIN10" MBRFormat /dev/disk4
Started erase on disk4 (CCCOMA_X64FRE_EN-US_DV9)
Unmounting disk
Creating the partition map
Waiting for partitions to activate
Formatting disk4s1 as MS-DOS (FAT32) with name WIN10
512 bytes per physical sector
/dev/rdisk4s1: 30401504 sectors in 1900094 FAT32 clusters (8192 bytes/cluster)
bps=512 spc=16 res=32 nft=2 mid=0xf8 spt=32 hds=255 hid=2048 drv=0x80 bsec=30431232 bspf=14845 rdcl=2 infs=1 bkbs=6
Mounting disk
Finished erase on disk4
```

This wil erase the USB drive, 
- format it with `ExFat`
- name the drive `"WINDOWS10"`
- give it an MBR (so it's bootable)


## Convert the ISO file

If the file image comes in a `.img` format, you will first need to convert it to a `.iso` format. 

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
- [diskutil man page](https://ss64.com/osx/diskutil.html)
- [Tips: How to make Windows 10 install media on macOS High Sierra](https://appleinsider.com/articles/18/01/18/tips-how-to-make-windows-10-install-media-on-macos-high-sierra)


