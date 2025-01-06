# VM Build Procedure

## Overview: 
This document explains the procedure for creating new VM’s on KVM hosts.

## Procedures
First pick the KVM where the VM will be built on. The selection depends on many factors, namely;

1. data center
2. Prod versus staging, versus dev.
3. Disk, CPU & RAM capacity on the KVM hosts


- Get the hostname. The name must be unused.
- Get the IP. The best way is to make sure the IP is unused based on DNS searches and trying to ping it. It is best to select the next available IP sequentially.
- Add the DNS entry for the new host.
- Generate a custom MAC address. For example, you can use the link below to accomplish this. [Random MAC Address Generator](https://www.hellion.org.uk/cgi-bin/randmac.pl) 
- Figure out the vlan ID and subnet ID’s. The best way to get these is by checking the ID’s used for other systems in the same network.
- Subnet IDs and VLAN IDs can be found by running this hammer command

```
[root@foreman01 ~]# hammer subnet list

---|--------------|--------------|----------------|---------------|---------|-----------|----------------
ID | NAME         | NETWORK ADDR | NETWORK PREFIX | NETWORK MASK  | VLAN ID | BOOT MODE | GATEWAY ADDRESS
---|--------------|--------------|----------------|---------------|---------|-----------|----------------
1  | MB2 - MGMT-4 | 10.36.35.0   | 24             | 255.255.255.0 | 35      | Static    | 10.36.35.1
6  | MB2 - MGMT-5 | 10.36.36.0   | 24             | 255.255.255.0 | 36      | Static    | 10.36.36.1
7  | MB2 - PROD-3 | 10.36.64.0   | 24             | 255.255.255.0 | 64      | Static    | 10.36.64.1
3  | MB2 - PROD-1 | 10.36.66.0   | 24             | 255.255.255.0 | 66      | Static    | 10.36.66.1
8  | MB2 - PROD-2 | 10.36.67.0   | 24             | 255.255.255.0 | 67      | Static    | 10.36.67.1
11 | MB2 - PROD-4 | 10.36.68.0   | 24             | 255.255.255.0 | 68      | Static    | 10.36.68.1
5  | MB2 - STG-1  | 10.36.192.0  | 24             | 255.255.255.0 | 192     | Static    | 10.36.192.1
9  | MB2 - STG-2  | 10.36.193.0  | 24             | 255.255.255.0 | 193     | Static    | 10.36.193.1
10 | MB2 - STG-3  | 10.36.194.0  | 24             | 255.255.255.0 | 194     | Static    | 10.36.194.1
12 | MB2 - STG-4  | 10.36.195.0  | 24             | 255.255.255.0 | 195     | Static    | 10.36.195.1
---|--------------|--------------|----------------|---------------|---------|-----------|----------------
```

- Also confirm the available networks on the KVM host.

```
[root@vm01 ~]# virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     no            yes
 vlan20               active     yes           yes
 vlan35               active     yes           yes
 vlan36               active     yes           yes
```

- Add the entry in Foreman. The values used below are simply examples.
```
hammer host create --name "db02" \
--hostgroup "FHM - PROD" \
--interface="primary=true,mac=48:df:37:1e:9c:a0,subnet_id=4,ip=10.33.64.42" \
--location "FHM" \
--organization "Variantyx" \
--ask-root-password yes
```

- Create VM in the KVM host. After the command is ran and presuming everything else above was correct, the VM will be created, booted up and installed from Foreman. The values used below are simply examples.

```
# virt-install \
--virt-type=kvm \
--name bioav3.prod \
--ram 32768 \
--vcpus=8 \
--os-variant=centos7.0 \
--pxe --network=network:vlan68,model=virtio --mac=e6:77:92:1c:01:77 \
--graphics=vnc \
--disk path=/var/lib/libvirt/images/bioav3.prod,size=1000,bus=virtio,format=qcow2
```

- You can monitor the build process by viewing the VM’s console with Wok by selecting the guest and then the console: https://vm02.mb2.prod.variantyx.com:8001/
- Once the build is completed make sure the VM is set to auto start when the KVM host is rebooted

```
virsh autostart {VM}
```

- Run the two playbooks below from boss01. The values used below are only to serve as examples.
```
ansible-playbook -i prod/hosts.prod.mb2.txt playbooks/deploy_authorized_keys.yml --limit db02.mb2.prod.variantyx.com --ask-pass	
ansible-playbook -i prod/hosts.prod.mb2.txt playbooks/standardBuild.yml --limit db02.mb2.prod.variantyx.com
```

## Virsh commands
List all VMs
```
virsh list --all
```

get basic info of the guest vm
```
virsh dominfo {VM}
```

List networks on the KVM host
```
[root@vm01 ~]# virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     no            yes
 vlan20               active     yes           yes
 vlan3                active     yes           yes
 vlan35               active     yes           yes
 vlan36               active     yes           yes
```

start, shutdown, and reboot a guest vm
```
virsh start {VM}
virsh shutdown {VM}
virsh reboot {VM}
```

Delete a vm guest
```
virsh destroy {VM}
virsh undefine {VM}
```

