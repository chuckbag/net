# Ansible Example Setup

## Overview:
Procedures for how to install Ansible (2.x) on a centos host.  

## Install 
Install the App

Install via yum. 
```bash
[root@boss01 ~]# yum install -y ansible
```

Confirming the install: 
```bash
[root@boss01 ~]# ansible --version
ansible 2.8.5
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.5 (default, Aug  7 2019, 00:51:29) [GCC 4.8.5 20150623 (Red Hat 4.8.5-39)]
[root@boss01 ~]#
```

## Setup SSH keys
In this example, we will have created a user `ansible` on the remote hosts who has sudo rights.  Then we will have to create the ssh keys for the “test” host, and copy it to that host.  

Doing this with the root user is a better solution, as you can run commands later unmanned, and without password issues.  


Create a pub/private key
```bash
[ansible@boss01 root]$ cd
[ansible@boss01 ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ansible/.ssh/id_rsa):
Created directory '/home/ansible/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ansible/.ssh/id_rsa.
Your public key has been saved in /home/ansible/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:N82VJHKoxV1uAe2lnwcd3n3vVmXCN7yF6l2tUQyihkY ansible@boss01.mb2.prod.variantyx.com
The key's randomart image is:
+---[RSA 2048]----+
|         ..o+++  |
|        E +oo+o+.|
|       . + . +BB+|
|        + oo oB+%|
|       .S.o o. BX|
|         . .. .oB|
|           . . =o|
|            . o o|
|               . |
+----[SHA256]-----+
[ansible@boss01 ~]$
```

Set permissions on the private keys
```bash
[ansible@boss01 ~]$ chmod 700 ~/.ssh
[ansible@boss01 ~]$ chmod 600 ~/.ssh/id_rsa
[ansible@boss01 ~]$
```


Make the .ssh dir on the remote client
```bash
[ansible@boss01 ~]$ ssh ansible@10.36.36.32 mkdir -p .ssh
The authenticity of host '10.36.36.32 (10.36.36.32)' cant be established.
ECDSA key fingerprint is SHA256:ZBK8t6eXCZEjKhEijs8TWnH3uUlQKDsNRJTMbp36MMY.
ECDSA key fingerprint is MD5:04:f2:d6:b8:ba:cb:b2:43:aa:dc:f7:7c:67:81:42:b2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.36.36.32' (ECDSA) to the list of known hosts.
ansible@10.36.36.32s password:
[ansible@boss01 ~]$

Copy the public key to the remote client.  
[ansible@boss01 ~]$ ssh-copy-id ansible@10.36.36.32
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/ansible/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ansible@10.36.36.32s password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ansible@10.36.36.32'"
and check to make sure that only the key(s) you wanted were added.

[ansible@boss01 ~]$
```

On the remote client, set the permissions, and ensure SELinux is set properly
```bash
$ ssh ansible@10.36.36.32
ansible@10.36.36.32s password:
Last login: Wed Mar 11 00:30:31 2020 from boss01.mb2.prod.variantyx.com
[ansible@temp ~]$
[ansible@temp ~]$ chmod 700 ~/.ssh
[ansible@temp ~]$ chmod 600 ~/.ssh/authorized_keys
[ansible@temp ~]$ restorecon -Rv ~/.ssh
[ansible@temp ~]$

Ensure you can ssh to the client properly: 
[ansible@boss01 ~]$ ssh ansible@10.36.36.32
Last login: Wed Mar 11 00:57:00 2020 from boss01.mb2.prod.variantyx.com
[ansible@temp ~]$ exit
logout
Connection to 10.36.36.32 closed.
[ansible@boss01 ~]$
```

## Configure
### Define servers to work with
Edit the host list that specifies all the servers we want to mess with. 


First backup the original, then open up the hosts file
```bash
[root@boss01 ~]# cp /etc/ansible/hosts /etc/ansible/hosts.bak
[root@boss01 ~]# vim /etc/ansible/hosts  
```

Then in the hosts file, add the `[group_name]` in brackets, and then the list of hosts for that group under it.
```
[tests]
10.36.36.32
```

Then confirm that you can see the list of hosts you just entered: 
```bash
[ansible@boss01 ~]$ ansible --list-hosts all
  hosts (1):
    10.36.36.32
[ansible@boss01 ~]$
```

And then confirm that you can connect to all of them, and run a basic ping command.  
```bash
[ansible@boss01 ~]$ ansible all -m ping
10.36.36.32 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
[ansible@boss01 ~]$
```

We can run the same command on the remote hosts via the user ansible, but also running the command under “sudo” (via -s): 

## References: 
- [Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html): Redhat, Mar 2020 
- [Securing OpenSSH/Use Public/Private Keys for Authentication](https://wiki.centos.org/HowTos/Network/SecuringSSH#head-9c5717fe7f9bb26332c9d67571200f8c1e4324bc): CentOS, Dec 2019
- [An Ansible Tutorial](https://serversforhackers.com/c/an-ansible-tutorial): Aug 2014



