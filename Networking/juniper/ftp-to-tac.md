# FTP to TAC

- [FTP to TAC](#ftp-to-tac)
  - [Overview:](#overview)
  - [Getting the basics off the box:](#getting-the-basics-off-the-box)
  - [Copy Logs to local Server:](#copy-logs-to-local-server)
    - [Leave CLI, and go to command prompt:](#leave-cli-and-go-to-command-prompt)
    - [Dir via CLI:](#dir-via-cli)
    - [Prepare files for transport:](#prepare-files-for-transport)
    - [Move the files to local bastion for transport:](#move-the-files-to-local-bastion-for-transport)
  - [FTP to jTAC:](#ftp-to-jtac)
  - [sFTP to jTAC:](#sftp-to-jtac)
  - [References:](#references)

## Overview: 

## Getting the basics off the box: 
Find the Serial number on the box (and the model version): 
```
> show chassis hardware
```

Find the running OS version: 
```
> show version
```

## Copy Logs to local Server:

### Leave CLI, and go to command prompt: 
```bash
user@juniper> start shell user root
```

Then enter in the admin's password

### Dir via CLI:
find the log file you are looking for:
```bash
> file show /var/log/messages?
Possible completions:
  <filename>           Filename to show
  /var/log/messages    Size: 721268, Last changed: Jun 08 01:12:25
  /var/log/messages.0.gz  Size: 153687, Last changed: Jun 03 16:10:00
  /var/log/messages.1.gz  Size: 89630, Last changed: Jun 03 16:09:00
  /var/log/messages.2.gz  Size: 154216, Last changed: Jun 03 15:59:00
  /var/log/messages.3.gz  Size: 219874, Last changed: Jun 03 15:58:00
  /var/log/messages.4.gz  Size: 55803, Last changed: Jun 03 15:35:00
  /var/log/messages.5.gz  Size: 72823, Last changed: Jun 03 15:33:00
  /var/log/messages.6.gz  Size: 259442, Last changed: Jun 03 15:32:00
  /var/log/messages.7.gz  Size: 36699, Last changed: Jun 03 15:30:00
  /var/log/messages.8.gz  Size: 228100, Last changed: Jun 03 15:05:00
  /var/log/messages.9.gz  Size: 78782, Last changed: Jun 03 14:18:00
{master}
cmercier@lhr-csw04a-re0>
```

### Prepare files for transport:
This can be very helpful if either there are many files, the files are large in size, or the rights on the files will not let you move them.

tar all the message files together from the cli and then confirm
```bash
> file archive source /var/log/messages* destination /var/log/messages-archive.tar
/usr/bin/tar: Removing leading `/' from member names

{master}
>
> file list detail /var/log/messages-archive.tar
-rw-------  1 Read-Write-Admin wheel   4177920 Jun 8  01:14 /var/log/messages-archive.tar
total 1

{master}
>
```

Or tar.gz the files from the shell:
```bash
cmercier@lhr-csw04a-re0> start shell
%
% tar -cvzf /var/log/archive-messages.tar.gz /var/log/messages*
tar: Removing leading `/' from member names
var/log/messages
var/log/messages.0.gz
var/log/messages.1.gz
var/log/messages.2.gz
var/log/messages.3.gz
var/log/messages.4.gz
var/log/messages.5.gz
var/log/messages.6.gz
var/log/messages.7.gz
var/log/messages.8.gz
var/log/messages.9.gz
% ls -lfh /var/log/archive-messages.tar.gz
-rw-r--r--  1 Read-Write-Admin  wheel   1.4M Jun  8 17:07 /var/log/archive-messages.tar.gz
```

### Move the files to local bastion for transport:

copy the tarball to a local server,
```bash
> file copy /var/log/messages-archive.tar chuckbag@10.218.73.6:/home/chuckbag/down/.
```

or pull the files to the local server:
```bash
[chuckbag ~]$ scp chuckbag@MX-1:/var/log/messages down/.
AUTHORIZED ACCESS ONLY - This system is the property of ME disconnect IMMEDIATELY if your are not an authorized user.
chuckbag@MX-1's password:
messages                                                                                         100%  476KB 475.8KB/s   00:00
[chuckbag ~]$
```

## FTP to jTAC:
Upload files to your ticket by going to the jTAC Case Management Home page

## sFTP to jTAC:
Log into the SFTP server: (user/pass = anonymous/anonymous)
```bash
unix:~> sftp anonymous@sftp.juniper.net
Connecting to sftp.juniper.net...
The authenticity of host 'sftp.juniper.net (66.129.230.52)' can't be established.
RSA key fingerprint is 8b:6b:36:94:ea:6d:92:55:bb:1f:80:3e:54:ea:4d:30.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'sftp.juniper.net,66.129.230.52' (RSA) to the list of known hosts.
anonymous@sftp.juniper.net's password: [ENTER “anonymous” for password]
```

Make a directory for your case uploads. These directories are removed after one (1) day, and can be re-created as needed.
```bash
sftp> mkdir pub/incoming/2012-0321-0722
```

If the directory already exists, you may see an error message such as this:
```bash
sftp> mkdir pub/incoming/2012-0321-0722
Couldnt create directory: Failure
```

Change directory to your case directory:
```bash
sftp> cd pub/incoming/2012-0321-0722
```

If the directory doesn’t exist, you may see an error message such as this:
```bash
sftp> cd pub/incoming/2012-0321-0722
Couldnt canonicalise: No such file or directory
```

You can list your case directory, as shown below, but be aware that the files are removed from your case directory minutes after the upload completes:
```bash
sftp> ls
sftp> dir
sftp>
```

Upload your file(s):
```bash
sftp> put bigfile.enc
Uploading bigfile.enc to /pub/incoming/2012-0321-0722/bigfile.enc
bigfile.enc 6% 356MB 10.8MB/s 07:26 ETA

….
sftp> put bigfile.enc
Uploading bigfile.enc to /pub/incoming/2012-0321-0722/bigfile.enc
bigfile.enc 100% 5158MB 10.8MB/s 07:58
sftp> ls -l 
-rw-r--r-- 1 30000 30000 5409027850 Mar 19 18:39 bigfile.enc
```

You can list the directory content only for a short time after the upload completes because the file is moved on the server to a location where it is not accessible to SFTP users.

Here is the same listing two minutes later:
```bash
sftp> ls -l
sftp>
```

## References:
- [Uploading Files](https://www.juniper.net/customers/csc/help/upload_files.jsp): TAC's long version of this.
- [How to upload large files to a JTAC Case](http://kb.juniper.net/InfoCenter/index?page=content&id=KB23337): KB23337