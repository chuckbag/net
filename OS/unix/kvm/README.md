# KVM

- [Install KVM](install.md): How to build a KVM server
- [KVM on CentOS 7](kvm.md): Setting up KVM and managing it with virt-manager over xwindows
- KVM/Kimchi/Wok: a significantly more advanced and gui kvm management solution. 
    - [install](install-wok.md): Getting the basic's up and working

## Viewing Running Services

View all the running virtual servers
```bash
[MRO][root@vm02 ~]# virsh list
 Id Name                 State
----------------------------------
  4 gate01               running
  5 db01                 running
  6 gate03               running
  7 web01                running
  8 db02                 running
  9 web02                running
 10 gate02               running
 25 rmq01                running
 26 rmq02                running
 27 rmq03                running
 28 tds01                running
```

View all VM's including the ones that are not running
```bash
[MRO][root@vm02 ~]# virsh list --all
 Id Name                 State
----------------------------------
  4 gate01               running
  5 db01                 running
  6 gate03               running
  7 web01                running
  8 db02                 running
  9 web02                running
 10 gate02               running
 25 rmq01                running
 26 rmq02                running
 27 rmq03                running
 28 tds01                running
```
