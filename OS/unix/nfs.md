# nfs

- [nfs](#nfs)
  - [For the Server](#for-the-server)
    - [install nfs](#install-nfs)
    - [enable services](#enable-services)
    - [setup the exports file](#setup-the-exports-file)
    - [Restart services to enable changes](#restart-services-to-enable-changes)
  - [For the Client](#for-the-client)
    - [Install NFS](#install-nfs-1)
    - [Create the directories that it will mount to](#create-the-directories-that-it-will-mount-to)
    - [Mount the volumes](#mount-the-volumes)
    - [Test](#test)
  - [References](#references)

## For the Server

### install nfs
install the necessary application to serve nfs mounts
```bash
yum install nfs-utils
```

### enable services
run the following to get everything running properly
```bash
systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
```

### setup the exports file
Modify the file: 
```bash
vim /etc/exports
```

you can present read/write (rw) volumes and read only (ro): 
```bash
# group1:
    /mnt/driveA 10.33.64.100(rw,sync,no_root_squash,no_all_squash)
# group2:
    /mnt/driveB 10.33.64.101(ro,sync,no_root_squash,no_all_squash)
```

### Restart services to enable changes
```bash
systemctl restart nfs-config
systemctl restart nfs-server
```


## For the Client
### Install NFS
```bash
yum install nfs-utils
```

### Create the directories that it will mount to
Create the directories on your client where you will want the mount points to be bound to.  You need to create a real directory on your client, and then you can tell the host that thats where the nfs mount will appear at. 
```bash
mkdir -p /mnt/driveA
mkdir -p /mnt/driveB
```

### Mount the volumes
The following commands will take the presented nfs mounts and bind them to the directories that you created above
```bash
mount -t nfs 10.33.64.100:/mnt/driveA /mnt/driveA
mount -t nfs 10.33.64.101:/mnt/driveB /mnt/driveB
```

### Test
Always good to confirm everything is working properly by testing if you can connect to the mounts
```bash
df -kh

touch /mnt/driveA/tmp.txt
touch /mnt/driveB/tmp.txt

ll /mnt/driveA/tmp.txt
ll /mnt/driveB/tmp.txt

rm -f /mnt/driveA/tmp.txt
rm -f /mnt/driveB/tmp.txt

umount 10.33.64.100:/mnt/driveA
umount 10.33.64.101:/mnt/driveB

df -kh
```

## References
- [CONFIGURING THE NFS SERVER](https://www.google.com/url?q=https%3A%2F%2Faccess.redhat.com%2Fdocumentation%2Fen-us%2Fred_hat_enterprise_linux%2F7%2Fhtml%2Fstorage_administration_guide%2Fnfs-serverconfig&sa=D&sntz=1&usg=AFQjCNF9OZib_sVMsP2NIxLL_O1naLw4LQ): 