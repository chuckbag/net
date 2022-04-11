# Example Kickstart config file


```
# -----------------------------------------------------
# COMMAND SECTION

# Action
install

# System authorization information
auth --enableshadow --passalgo=sha512

# Accept Eula
eula --agreed

# when install is complete, reboot rather then "halt"ing or "shutdown"
reboot

### Network Installation ###

# installation media
url --url="ftp://198.18.3.20/pub/"

# Run the Setup Agent on first boot (vda is for VMs, sda would be physical)
firstboot --enable
ignoredisk --only-use=vda

# Keyboard layouts
keyboard --vckeymap=us --xlayouts='us'

# System language
lang en_US.UTF-8

###

# Network information
network --activate --bootproto=dhcp --device=eth0 --noipv6
network  --hostname=localhost.cmed.us


# Accounts: 
rootpw --iscrypted $6$XOiY55U37GmMS/M$wzBpgJmn2bAkuFgxYSFrmQW/wKyL5yWSMOR7WYrE.XFIA94M0wWmc1oKLW8mgiQ0mTrLH0xnYLyblU7Xr1
user --name=cmercier --password=$6$78U6jYzzuPzL3Mm$1GOtTKJyfnY5QFbRI/zR1g7zYhbx63IQJD9PhHN/XUvTpLZJXFa.1G.SbXckAgoEeKaKmi1ej.8G76Cek1 --iscrypted --gecos="cmercier" --groups=wheel --uid=100

# System services
services --enabled="chronyd,sshd"

# System timezone
timezone Etc/GMT --Utc --ntpservers=0.north-america.pool.ntp.org,1.north-america.pool.ntp.org

# System bootloader configuration (zerombr= don't prompt, run headless)
bootloader --append=" crashkernel=auto" --location=mbr --boot-drive=vda
autopart --type=lvm
zerombr

# Partition clearing information
clearpart --all 

#firewalling
firewall --enable --ssh 

# -----------------------------------------------------
# PACKAGES SECTION

%packages
@^minimal
@core
chrony
kexec-tools
net-tools
vim
curl
rsync
bind-utils
net-snmp-utils


%end

%addon com_redhat_kdump --enable --reserve-mb='auto'

%end

%anaconda
pwpolicy root --minlen=6 --minquality=1 --notstrict --nochanges --notempty
pwpolicy user --minlen=6 --minquality=1 --notstrict --nochanges --emptyok
pwpolicy luks --minlen=6 --minquality=1 --notstrict --nochanges --notempty
%end
```
