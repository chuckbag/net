# Enable TFTP

## Enable TFTP service: 
```
$ sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
Password:
$ sudo launchctl start com.apple.tftpd
$ 
$ ps -ef | grep tftp
1207504055 30876 27613   0 12:21PM ttys002    0:00.00 grep tftp
$ 
```

Confirm its up and running with nmap: 
```
$ sudo nmap -sU -p 69 localhost

Starting Nmap 7.12 ( https://nmap.org ) at 2017-02-08 13:10 EST
Nmap scan report for localhost (127.0.0.1)
Host is up.
Other addresses for localhost (not scanned): ::1
PORT   STATE         SERVICE
69/udp open|filtered tftp

Nmap done: 1 IP address (1 host up) scanned in 2.11 seconds
```

## Default path for tftp: 
Files available to tftp are located here:  
```
/private/tftpboot/
```

ensure that the directory is readable 
```
$ sudo chmod 777 /private/tftpboot
Password:
$ chmod a-x /private/tftpboot/*
$ ll /private/tftpboot/
total 48496
-rw-rw-rw-@ 1 cmercier  wheel    24M Feb  8 12:11 asa846-k8.bin
$ 
```

Remember that tftp can only download files from the repo, or replace them.  (You can't add new files, so if required, you need to touch the file first and then you can download the new file.)

## Disable TFTP service: 
```
$ sudo launchctl unload -F /System/Library/LaunchDaemons/tftp.plist
```

## References: 
- [Using the OS X built-in tftp server](https://weezey.com/2011/07/using-os-x-built-in-tftp-server/)   