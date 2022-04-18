             
```
# -------------------------------------------------------------------------- #
# The install procedures for taco.  (A filer server system)


# -------------------------------------------------------------------------- #
# 1. Initial boot

net-setup
    select: eth0
    select: My network is wired
    select: Use DHCP to auto-detect my network settings
passwd
/etc/init.d/sshd start
ifconfig
    Continue by logging in remotely.

# -------------------------------------------------------------------------- #
# 2. Install Gentoo:

# 2.1 Prepair the disks:
fdisk /dev/hda
    p
        n, p, 1, [enter], +32M          #boot
        n, p, 2, [enter], [enter]       #root
        a, 1                            #set boot to bootable
        w                               #write changes

fdisk /dev/hdb
    p
        n, p, 1, [enter], +4000M        #swap
        n, p, 2, [enter], [enter]       #log
        t, 2, 82                        #set swap drive to swap type
        w                               #write changes
   
fdisk /dev/hde
    p
        n, p, 1, [enter], [enter]        #Disk1
        w

fdisk /dev/hdf
    p
        n, p, 1, [enter], [enter]        #Disk2
        w


#
# The 80G drive will host the boot and root partitions.  The small 10G disk
# will handle swap and log file info to speed up the performance on the root
# drive. 
# The primary and secondary filer disks will be on the 250G drives, and they
# will not hold any OS information other then backup data.
#
#       hda           hdb             hde         hdf  
#      80GB           10GB           250GB       250GB   
#    /-------\      /-------\      /-------\   /-------\
#    | boot  | 32M  |       |      |       |   |       |
#    +-------+      | swap  | 4G   |       |   |       |
#    |       |      |       |      | disk1 |   | disk2 |
#    | root  | 79G  +-------+      |       |   |       |
#    |       |      |       |      |       |   |       |
#    |       |      |       |      |       |   |       |
#    |       |      |  log  | 6G   |       |   |       |
#    |       |      |       |      |       |   |       |
#    |       |      |       |      |       |   |       |
#    \-------/      \-------/      \-------/   \-------/



mke2fs /dev/hda1
mke2fs -j /dev/hda2

mke2fs -j /dev/hdb2
mkswap /dev/hdb1
swapon /dev/hdb1

mount /dev/hda2 /mnt/gentoo
mkdir /mnt/gentoo/boot
mount /dev/hda1 /mnt/gentoo/boot

# 2.2 Download Stages and Portage
cd /mnt/gentoo
links http://www.gentoo.org/main/en/mirrors.xml
    select: CSU Open Source Lab                   
    select: releases/
    select: x86/
    select: 2007.0/
    select: stages/
    select to download: stage3-x86-2007.0.tar.bz2 
    select (at top): Gentoo
    select: snapshots/
    select: 2006.1
    select to download: portage-20070517.tar.bz2  
    select: q, yes

tar xvjpf stage3-*.tar.bz2
tar xvjf /mnt/gentoo/portage-20070517.tar.bz2 -C /mnt/gentoo/usr
                

# 2.3 Optimise and mount
vim /mnt/gentoo/etc/make.conf
    MAKEOPTS="-j2"
  


mirrorselect --deep --servers 5 -o >> /mnt/gentoo/etc/make.conf
mirrorselect -i -r -o >> /mnt/gentoo/etc/make.conf
    select: rsync://rsync.namerica.gentoo.org/gentoo-portage

cp -L /etc/resolv.conf /mnt/gentoo/etc/resolv.conf
mount -t proc none /mnt/gentoo/proc
mount -o bind /dev /mnt/gentoo/dev
chroot /mnt/gentoo /bin/bash
env-update
source /etc/profile
export PS1="(chroot) $PS1"

# 2.4 Configuring Portage
emerge --sync; emerge portage; emerge vim; emerge screen
dispatch-conf
vim /etc/make.conf
    
USE="-gtk -gnome -qt3 -qt4 -kde -X
crypt ssl perl samba png quicktime apache2 swat"


vim /etc/locale.gen
    en_US ISO-8859-1
    en_US.UTF-8 UTF-8
locale-gen
cp /usr/share/zoneinfo/EST /etc/localtime

USE="-doc symlink" emerge gentoo-sources

# 2.5 Install Kernel (manual)
cd /usr/src/linux; make menuconfig
    Code maturity level options --->
      [*] Prompt for development and/or incomplete code/drivers
    Processor type and features --->
      [*] Processor family (Athlon/Duron/K7)  --->
        (*) Pentumum-III/Celeron(Coppermine)/Pentimum-III Xeon
    Device Drivers --->
      ATA/ATAPI/MFM/RLL support --->
        [*] PI IDE chipset support
          [*] Generic PCI bus-master DMA support
          [*]   Use PCI DMA by default when available
      USB Support --->
        <*>   USB Human Interface Device (full HID) support
    File systems --->
      <*> Ext3 journalling file system support
      Pseudo Filesystems --->
        [*] /proc file system support
        [*] Virtual memory file system support (former shm fs)
make && make modules_install
ls -l /usr/src/linux
    Note the kernel version and update the following command if needed
cp arch/i386/boot/bzImage /boot/kernel-2.6.18-gentoo-r4

# 2.6 Backup Kernel Install (genkernel)
emerge genkernel; genkernel all   <------------------------------  !!!!!
ls /boot/kernel* /boot/initramfs*

/boot/initramfs-genkernel-x86-2.6.20-gentoo-r8 
/boot/kernel-genkernel-x86-2.6.20-gentoo-r8


   Note this output for the later Grub install

vim /etc/modules.autoload.d/kernel-2.6
    e100
    e1000

# 2.7 Filesystem configuring
vim /etc/fstab
    # ------------------------------------------------------------ #
    # A drive with boot and root:
    /dev/hda1   /boot        ext2    defaults,noatime     1 2
    /dev/hda2   /            ext3    noatime              0 1
   
    # B drive with swap and log:
    /dev/hdb1   none         swap    sw                   0 0
    /dev/hdb2   /var/log     ext3    noatime              0 1

    # E and F are the Filer disks:
    /dev/hde1   /disk1        reiserfs    noatime              0 1
    /dev/hdf1   /disk2        reiserfs    noatime              0 1

    # ------------------------------------------------------------ #
    # Other standard mounts:

    proc        /proc        proc    defaults             0 0
    shm         /dev/shm     tmpfs   nodev,nosuid,noexec  0 0

    /dev/cdrom  /mnt/cdrom   auto    noauto,user          0 0


vim /etc/conf.d/hostname
    HOSTNAME="taco"
vim /etc/conf.d/net

config_eth0=( "192.168.1.30 netmask 255.255.255.0 broadcast 192.168.1.255" )
routes_eth0=("default via 192.168.1.1")

dns_domain_eth0="cmed.us"
dns_servers_eth0="192.168.1.27 "
dns_search_eth0="cmed.us"

rc-update add net.eth0 default
vim /etc/hosts
    127.0.0.1       localhost taco taco.cmed.us
passwd
vim /etc/rc.conf
    #EDITOR="/bin/nano"
    EDITOR="/usr/bin/vim"
vim /etc/conf.d/keymaps
    SET_WINDOWKEYS="yes"
vim /etc/conf.d/clock
    CLOCK="local"
emerge syslog-ng slocate vixie-cron
rc-update add vixie-cron default
rc-update add syslog-ng default

# 2.8 Install Bootloader:
emerge grub; vim /boot/grub/grub.conf
    default 0
    timeout 30
    splashimage=(hd0,0)/boot/grub/splash.xpm.gz

    # Partition where the kernel image (or operating system) is located
    title=Gentoo Linux 2.6.18-r4 (genkernel)
    root (hd0,0)
    kernel /boot/kernel-genkernel-x86-2.6.20-gentoo-r8 root=/dev/ram0 init=/linuxrc ramdisk=8192 real_root=/dev/hda2 udev    
    initrd /boot/initramfs-genkernel-x86-2.6.20-gentoo-r8



grub
    root (hd0,0)
    setup (hd0)
    quit        

rc-update add sshd default

# 2.9 Reboot:
exit
cd
umount /mnt/gentoo/boot /mnt/gentoo/dev /mnt/gentoo/proc /mnt/gentoo
reboot

# -------------------------------------------------------------------------- #
# 3. Server side application installs: Emerge apps and create users.


emerge sync
emerge perl tcpdump bind-tools samba nfs-utils ntp rcs rsnapshot


useradd -u 1000 -G users,wheel,audio -s /bin/bash mowgli

passwd mowgli

useradd -u 1001 -G users,wheel,audio -s /bin/bash bets

passwd bets


# -------------------------------------------------------------------------- #
# 4. Install Samba:

# 4.1 Setup system revision control on files that are non-standard.  (So updates
#     won't overwrite them.)

mkdir -p /root/RC-configs/RCS
ln /etc/make.conf /root/RC-configs/make.conf
ln /etc/locale.gen /root/RC-configs/locale.gen
ln /etc/localtime /root/RC-configs/localtime
ln /etc/modules.autoload.d/kernel-2.6 /root/RC-configs/kernel-2.6
ln /etc/fstab /root/RC-configs/fstab
ln /etc/conf.d/hostname /root/RC-configs/hostname
ln /etc/conf.d/net /root/RC-configs/net
ln /etc/hosts /root/RC-configs/hosts
ln /etc/rc.conf /root/RC-configs/rc.conf
ln /etc/conf.d/keymaps /root/RC-configs/keymaps
ln /etc/conf.d/clock /root/RC-configs/clock

cd
vim .bashrc
    # for shell usage:
    alias ll='ls -lFh --color'

    # for RCS
    alias cin='ci -u'
    alias cout='co -l'

source .bashrc

cd RC-configs
cin *


# 4.2 Setup NFS Services:

#     Reference: http://www.nit.gwu.edu/~chuck/nfs/gentoo-nfs.html

vim /etc/exports
    # Local Volumes to share via NFS:
    /disk1/share 192.168.1.0/24(rw,sync)
    /disk2/mp3 192.168.1.0/24(rw,sync)

/etc/init.d/nfs restart

rc-update add nfs default


emerge ethtool

ethtool -s eth0 speed 100 duplex full autoneg off


# 4.3 Setup Samba Services:
# Basic Samba Setup
emerge samba xinetd

rc-update add samba default
rc-update add xinetd default

vim /etc/xinetd.d/swat
    service swat
    {
            port            = 901
            socket_type     = stream
            wait            = no
            only_from       = 192.168.1.0
            user            = root
            server          = /usr/sbin/swat
            log_on_failure += USERID
            disable         = no
    }

/etc/init.d/xinetd start
cp /etc/samba/smb.conf.example /etc/samba/smb.conf


# 4.3.1 add users:

smbpasswd -a root <add_password>
smbpasswd -a bets <add_password>
smbpasswd -a mowgli <add_password>

# 4.3.2 configure samba

http://192.168.1.30:901



<global>

    workgroup: CMED

    netbios name: TACO

    server string: Samba Server %v

    interfaces: lo eth0



    security: user

    encrypt passwords: yes

    client schannel: auto

    server schannel: auto

    map to guest: Bad User

    guest account: nobody

    log file: /var/log/samba/log.%m
    max log size: 50

    socket options: TCP_NODELAY SO_RCVBUF=8192 SO_SNDBUF=8192
   
    os level: 20
    preferred master: Auto
    local master: Yes
    domain master: Auto

    dns proxy: No
    wins support: No

    winbind nss info: template

<shares>
    [choose share] <homes> {select}

    Base Options
    comment: Home Directories
    path: /disk1/share

    Security Options
    valid users: %S
    read only: No
    guest ok: No
   
    Browse Options
    browseable: No
   
    EventLog Options
    available: Yes

    [commit changes]

<share>
    [Create Share] mp3 {select}

    Base Options
    comment: Music Share
    path: /disk2/mp3

    Security Options
    read only: Yes
    guest ok: Yes
   
    Browse Options
    browseable: Yes
   
    EventLog Options
    available: Yes

    [commit changes]



/etc/init.d/samba start

# 4.4  then connect to the shares via the url's:

\\192.168.1.30\mp3

\\192.168.1.30\{username}


# 4.5 Ref:
http://gentoo-wiki.com/HOWTO_Setup_Samba#Server

http://www.gentoo.org/doc/en/quick-samba-howto.xml#doc_chap3

http://safari.oreilly.com/0596007698/samba3-PREFACE-2#X2ludGVybmFsX1NlY3Rpb25Db250ZW50P3htbGlkPTA1OTYwMDc2OTgvc2FtYmEzLUNIUC00LVNFQ1QtNg==







# -------------------------------------------------------------------------- #
# 5. Setup rSnapShot Services:



# 5.? Ref:
http://www.rsnapshot.org/


# -------------------------------------------------------------------------- #
# Appendix A1: Re-entering Oak via the boot CD:

swapon /dev/hda2
mount /dev/hda3 /mnt/gentoo
mount /dev/hda1 /mnt/gentoo/boot
mount -t proc none /mnt/gentoo/proc
mount -o bind /dev /mnt/gentoo/dev
chroot /mnt/gentoo /bin/bash
env-update
source /etc/profile
export PS1="(chroot) $PS1"

```

