# GNS3-ubuntu
To get GNS3 Installed properly on Ubuntu (9.10-10.10), follow these steps:

Make supporting directories:
```
sudo mkdir /opt/GNS3/Dynamips
sudo mkdir /opt/GNS3/IOS
sudo mkdir /opt/GNS3/Project
sudo mkdir /opt/GNS3/Cache
sudo mkdir /opt/GNS3/tmp

sudo chmod o+rw -R /opt/GNS3/tmp
sudo chmod o+rw -R  /opt/GNS3/IOS
sudo chmod o+rw -R  /opt/GNS3/Project
```

Install GNS3 and supporting requirements:
```
sudo apt-get install dynagen python-qt4
sudo apt-get install qt4-dev-tools

cd /opt
sudo wget http://space.dl.sourceforge.net/project/gns-3/GNS3/0.7.3/GNS3-0.7.3-src.tar.bz2
sudo tar -xjvf GNS3-0.7.3-src.tar.bz2 && rm GNS3-0.7.3-src.tar.bz2
sudo mv GNS3-0.7.3-src /opt/GNS

cd /opt/GNS3/Dynamips

# for 32-bit
sudo wget http://www.ipflow.utc.fr/dynamips/dynamips-0.2.8-RC2-x86.bin
sudo chmod +x ./dynamips-0.2.8-RC2-x86.bin

# for 64-bit
sudo wget http://www.ipflow.utc.fr/dynamips/dynamips-0.2.8-RC2-amd64.bin
sudo chmod +x ./dynamips-0.2.8-RC2-amd64.bin
sudo apt-get install qemu
```

Create menu icons:
- right-click on the menubar's "application" menu, and select "edit to menu"
- select the directory you want, and click "New Item"
- Name it GNS3
- the command section type in: python "/opt/GNS3/gns3"

Open GNS3 make the following modifications:
- under the "edit" tab , select "preferences",
- in the preferences window, in the left menu, choose the General section, select the "Terminal Settings" tab, and modify the terminal command to be: `gnome-terminal -t %d -e 'telnet %h %p' > /dev/null 2>&1 &`
- Within the preferences window, under the "General Settings" tab,
    - change the Paths, "project directory" to `/opt/GNS3/Project` 
    - and the "image directory to `/opt/GNS3/IOS` 
- Still within the Preferences window, select the Dynamips left menu option, and
    - in the "Executable path" type in `/opt/GNS3/Dynamips/dynamips-0.2.8-RC2-x86.bin` 
    - in the "working directory", type in `/opt/GNS3/tmp`
- Still within the Preferences window, select the Capture left menu option, and in the "Working directory for capture files" enter `/opt/GNS3/Project`
- Still within the Preferences window, select the Qemu left menu option, and 
    - within the "General Settings" tab, under "Path to qemu" select: `/usr/bin/qemu`
    - And also within the "General Settings" tab, under "Path to qemu-ing" select: `/usr/bin/qemu-img`

## References:
- [(Tutorial) How To Install GNS3 on Ubuntu 9.10/10.04/10.10](http://www.gns3.net/phpBB/topic2217.html?sid=ae9cf7f268c8e8adc71e9180a48e1ac3), Jun 20, 2010, by Glavin
- [How To Install GNS3 on Ubuntu 10.04 LTS](http://networkingtips-tricks.blogspot.com/2010/09/how-to-install-gns3-on-ubuntu-1004-lts.html), Sep 2010, By Naresh