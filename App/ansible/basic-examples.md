# Ansible Basic Examples

- [Ansible Basic Examples](#ansible-basic-examples)
  - [Overview:](#overview)
  - [Run as](#run-as)
  - [Define List of Machines](#define-list-of-machines)
  - [Setup SSH Keys](#setup-ssh-keys)
    - [On the Ansible Server](#on-the-ansible-server)
    - [On the Client Hosts](#on-the-client-hosts)
  - [Manual Shell Command (ad-hoc)](#manual-shell-command-ad-hoc)
    - [Connectivity Check](#connectivity-check)
    - [Shell Command](#shell-command)
    - [Using Modules in ad-hoc](#using-modules-in-ad-hoc)
      - [Yum module](#yum-module)
      - [File module (add directory)](#file-module-add-directory)
      - [Copy module](#copy-module)
      - [File module (delete directory and contents)](#file-module-delete-directory-and-contents)
  - [Changes via Playbooks](#changes-via-playbooks)
    - [Using Playbooks to run Multiple Installs (yum module)](#using-playbooks-to-run-multiple-installs-yum-module)
    - [Using Playbooks to Install, configure, and enable (including handlers)](#using-playbooks-to-install-configure-and-enable-including-handlers)
  - [References](#references)

## Overview: 
This is just a series of simple steps that go over some of the more basic ideas within Ansible.  

All of this is being run on ansible version 2
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

## Run as
While I would like to run everything as user ansible, doing so means that when you connect to other workstations, you need to enter in the su password to be able to do anything privileged. Because of this, you end up storing a root password in a file in a method that I didn’t see as much more secure, and also not something that others were doing.   

## Define List of Machines
Create a file that includes all of the hosts that you want to manage.  An example file is in `/etc/ansible/hosts` and you can mark that up, or create a new one in a different location.  For simplicity, let's create a new file and keep it in a new directory in the path `/root/ansible/hosts` , and then edit the file.    
```bash
[root@boss01 ~]# pwd
/root
[root@boss01 ~]# mkdir ansible
[root@boss01 ~]# cp /etc/ansible/hosts /root/ansible/
[root@boss01 ~]# vim /root/ansible/hosts
```

The example document has many different examples of adding hosts.  In the following example, we have one ungrouped host (10.33.128.150), and one that is part of the “tests” group.  
```
10.33.128.150

[tests]
10.36.36.32
```

I can confirm these with the  command, either by viewing all the hosts
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts --list-hosts all
  hosts (2):
    10.33.128.150
    10.36.36.32
[root@boss01 ~]#
```

Or just looking at the group “tests” hosts
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts --list-hosts tests
  hosts (1):
    10.36.36.32
[root@boss01 ~]#
```

Note the -i /root/ansible/hosts argument specifies the exact host “inventory” to use, and the last argument all shows all the hosts, and tests shows only the hosts in the “tests” group.


## Setup SSH Keys
The main way ansible logs into remote hosts is through stored public keys.  Since we’re wanting to log into multiple hosts root accounts, you will need to do this on both the ansible’s root account, and the remote hosts’ root accounts.  


### On the Ansible Server
Create a pub/private key
```bash
[root@boss01 ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:jmECCQpDRSNd741um8QDKyBrsifucNuTaCEPz5vTNzQ root@boss01.mb2.prod.variantyx.com
The key's randomart image is:
+---[RSA 2048]----+
|=o++..           |
|oo.o. .          |
|. o    .         |
|   .  . o        |
|..  ..ooS.       |
|+.o  oE+         |
|+B.= +.B.        |
|=oO+= = +        |
|+*+o.o +         |
+----[SHA256]-----+
[root@boss01 ~]#
```

Set permissions on the private keys
```bash
[root@boss01 ~]# chmod 700 ~/.ssh
[root@boss01 ~]# chmod 600 ~/.ssh/id_rsa
[root@boss01 ~]#
```

Make the .ssh directory on each of the remote clients
```bash
[root@boss01 ~]# ssh root@10.36.36.32 mkdir -p .ssh
root@10.36.36.32s password:
[root@boss01 ~]# ssh root@10.33.128.150 mkdir -p .ssh
The authenticity of host '10.33.128.150 (10.33.128.150)' cant be established.
ECDSA key fingerprint is SHA256:Y0rpESfw77mlMtKkkmDXMAWqnWiZE+7awd4hpYaq0g0.
ECDSA key fingerprint is MD5:bf:ad:3e:d6:42:d2:c7:62:e7:3b:d8:cb:34:72:1b:4e.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.33.128.150' (ECDSA) to the list of known hosts.
root@10.33.128.150s password:
[root@boss01 ~]#
```

And Copy the public key to the remote clients
```bash
[root@boss01 ~]# ssh-copy-id root@10.36.36.32
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@10.36.36.32s password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@10.36.36.32'"
and check to make sure that only the key(s) you wanted were added.

[root@boss01 ~]# ssh-copy-id root@10.33.128.150
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@10.33.128.150s password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@10.33.128.150'"
and check to make sure that only the key(s) you wanted were added.

[root@boss01 ~]#
```

### On the Client Hosts
Just make sure that the permissions are setup, and that SELinux is setup properly. 
Log into each client, set the permissions, log out, and re-login to confirm everything works. 
```bash
[root@boss01 ~]# ssh root@10.33.128.150
Last login: Wed Apr 15 16:15:34 2020 from boss01.mb2.prod.variantyx.com
[root@test01 ~]# chmod 700 ~/.ssh
[root@test01 ~]# chmod 600 ~/.ssh/authorized_keys
[root@test01 ~]# restorecon -Rv ~/.ssh
[root@test01 ~]# exit
logout
Connection to 10.33.128.150 closed.
[root@boss01 ~]# ssh root@10.33.128.150
Last login: Wed Apr 15 16:29:09 2020 from boss01.mb2.prod.variantyx.com
[root@test01 ~]# exit
logout
Connection to 10.33.128.150 closed.
[root@boss01 ~]#
```

```bash
[root@boss01 ~]# ssh root@10.36.36.32
Last login: Wed Apr 15 15:29:52 2020 from vpnuser-10.fhm.prod.variantyx.com
[root@temp ~]# chmod 700 ~/.ssh
[root@temp ~]# chmod 600 ~/.ssh/authorized_keys
[root@temp ~]# restorecon -Rv ~/.ssh
[root@temp ~]# exit
logout
Connection to 10.36.36.32 closed.
[root@boss01 ~]# ssh root@10.36.36.32
Last login: Wed Apr 15 16:30:10 2020 from boss01.mb2.prod.variantyx.com
[root@temp ~]# exit
logout
Connection to 10.36.36.32 closed.
[root@boss01 ~]#
```

## Manual Shell Command (ad-hoc)
There are many ways to run commands on remote hosts, but the two most basic are the remote ping and remote shell methods. 

### Connectivity Check
This runs a simple check (not icmp ping) that confirms that you can talk to all the systems in your hosts list. 
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m ping

10.33.128.150 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python2.7"
    },
    "changed": false,
    "ping": "pong"
}
10.36.36.32 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
[root@boss01 ~]#
```

### Shell Command
You can run any command remotely with Ansible, but in this example, we will use the logger command, and have a message dropped to the remote host’s /var/log/messages file.  

Running the command, we defined the path to the hosts file with the `-i` flag, we state we want the command to run on **all** hosts, we want to use the shell module with the `-m shell` argument, and then we want the command **logger Confirmed Ansible Ran Command** to be run on the remote hosts.  
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m shell -a 'logger Confirmed Ansible Ran Command'
10.36.36.32 | CHANGED | rc=0 >>


 [WARNING]: Distribution centos 7.7.1908 on host 10.33.128.150 should use /usr/bin/python, but is using
/usr/bin/python2.7, since the discovered platform python interpreter was not present. See
https://docs.ansible.com/ansible/2.8/reference_appendices/interpreter_discovery.html for more information.

10.33.128.150 | CHANGED | rc=0 >>

[root@boss01 ~]#
```

Confirming that the messages were received on the remote hosts
```bash
[root@temp ~]# grep Ansible /var/log/messages
Apr 15 16:51:26 temp ansible-command: Invoked with creates=None executable=None _uses_shell=True strip_empty_ends=True _raw_params=logger Confirmed Ansible Ran Command removes=None argv=None warn=True chdir=None stdin_add_newline=True stdin=None
Apr 15 16:51:26 temp root: Confirmed Ansible Ran Command
[root@temp ~]#

[root@test01 ~]# grep Ansible /var/log/messages
Apr 15 16:51:26 test01 ansible-command: Invoked with creates=None executable=None _uses_shell=True strip_empty_ends=True _raw_params=logger Confirmed Ansible Ran Command removes=None argv=None warn=True chdir=None stdin_add_newline=True stdin=None
Apr 15 16:51:26 test01 root: Confirmed Ansible Ran Command
[root@test01 ~]#
```

### Using Modules in ad-hoc
In the section above, we used the shell “module” to run our remote commands.  We don’t need to do things that way, and in fact it's better to use the standard modules for the specific commands you need to run.  

There is a module for just about everything you need to do on a remote host.  The following are a few examples. 

#### Yum module
While you could run a shell command to run yum on a remote host, it’s easier to use the `yum` module.  

If you wanted to run a yum update on all your hosts, you could use the yum module, and add the variables `state=latest name=*`. 
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m yum -a 'state=latest name=*'
 10.33.128.150 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python2.7"
    },
    "changed": true,
    "changes": {
        "installed": [],
        "updated": [
            [
                "mongodb-org-shell",
                "4.2.5-1.el7.x86_64 from mongodb-org-4.2"
            ],
            [
                "mongodb-org-tools",
                "4.2.5-1.el7.x86_64 from mongodb-org-4.2"
            ],
            [
                "mongodb-org-mongos",
                "4.2.5-1.el7.x86_64 from mongodb-org-4.2"
            ],
            [
                "mongodb-org-server",
                "4.2.5-1.el7.x86_64 from mongodb-org-4.2"
            ]
        ]
    },
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: enabled_repos_upload, fastestmirror, package_upload, product-id,\n              : search-disabled-repos, subscription-manager\nLoading mirror speeds from cached hostfile\nResolving Dependencies\n--> Running transaction check\n---> Package mongodb-org-mong
[...]
}
10.36.36.32 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "Nothing to do here, all packages are up to date"
    ]
}
[root@boss01 ~]#
```

To install the application “net-snmp” on all your hosts using yum, you would do the same, but the argument would be `state=latest name=net-snmp`, where “name” is the app you want to install and “state” is the version.    
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m yum -a 'state=latest name=net-snmp'
10.36.36.32 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "changes": {
        "installed": [
            "net-snmp"
        ],
        "updated": []
    },
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: fastestmirror, product-id, search-disabled-repos, subscription-\n              : manager\nLoading mirror speeds from cached hostfile\nResolving Dependencies\n--> Running transaction check\n---> Package net-snmp.x86_64 1:5.7.2-43.
[...]
ibs.x86_64 0:3.4.0-8.20160601gitf9185e5.el7                       \n  net-snmp-agent-libs.x86_64 1:5.7.2-43.el7                                     \n  perl-Data-Dumper.x86_64 0:2.145-3.el7                                         \n\nComplete!\n"
    ]
}

10.33.128.150 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python2.7"
    },
    "changed": true,
    "changes": {
        "installed": [
            "net-snmp"
        ],
        "updated": []
    },
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: enabled_repos_upload, fastestmirror, package_upload, product-id,\n              : search-disabl
[...]
nager\n"
    ]
}
[root@boss01 ~]#
```

#### File module (add directory)
We can create a directory on remote hosts with the file module.  
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m file -a "path=/tmp/ansible mode=0777 state=directory"
10.36.36.32 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0777",
    "owner": "root",
    "path": "/tmp/ansible",
    "secontext": "unconfined_u:object_r:user_tmp_t:s0",
    "size": 4096,
    "state": "directory",
    "uid": 0
}

10.33.128.150 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python2.7"
    },
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0777",
    "owner": "root",
    "path": "/tmp/ansible",
    "secontext": "unconfined_u:object_r:user_tmp_t:s0",
    "size": 4096,
    "state": "directory",
    "uid": 0
}
[root@boss01 ~]#
```


#### Copy module 
Yum has a module for easily copying files to multiple locations.  We define the local source of the file, and where you want it placed on all the hosts, and it pushes them.  
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m copy -a "src=/root/example_text_file.txt dest=/tmp/ansible"
10.36.36.32 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "da39a3ee5e6b4b0d3255bfef95601890afd80709",
    "dest": "/tmp/ansible/example_text_file.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "d41d8cd98f00b204e9800998ecf8427e",
    "mode": "0644",
    "owner": "root",
    "secontext": "unconfined_u:object_r:admin_home_t:s0",
    "size": 0,
    "src": "/root/.ansible/tmp/ansible-tmp-1586974679.02-116067971432933/source",
    "state": "file",
    "uid": 0
}

10.33.128.150 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python2.7"
    },
    "changed": true,
    "checksum": "da39a3ee5e6b4b0d3255bfef95601890afd80709",
    "dest": "/tmp/ansible/example_text_file.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "d41d8cd98f00b204e9800998ecf8427e",
    "mode": "0644",
    "owner": "root",
    "secontext": "unconfined_u:object_r:admin_home_t:s0",
    "size": 0,
    "src": "/root/.ansible/tmp/ansible-tmp-1586974679.0-232800761885185/source",
    "state": "file",
    "uid": 0
}
[root@boss01 ~]#
```

#### File module (delete directory and contents)
We can use the file module again to delete the directory we just made, as well as the contents in it.  
```bash
[root@boss01 ~]# ansible -i /root/ansible/hosts all -m file -a "path=/tmp/ansible state=absent"
10.36.36.32 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "path": "/tmp/ansible",
    "state": "absent"
}

10.33.128.150 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python2.7"
    },
    "changed": true,
    "path": "/tmp/ansible",
    "state": "absent"
}
[root@boss01 ~]#
```

## Changes via Playbooks
In the previous chapter “Manual Shell Commands”, we went through a couple of ways to make specific changes to systems by making single commands on the console.  Playbooks allow us to make libraries of all our commands, and then group them and run multiple commands from one run.  


Playbooks are simple yaml files that list all of the things you want to do together.  In this example, we’ll make a list of applications that we want either installed or updated on all management systems.  


### Using Playbooks to run Multiple Installs (yum module)
In this playbook example, we will install a bunch of different things, and make sure they are up to date.  

Creating the file in the “ansible” directory
```bash
[root@boss01 ~]# vim /root/ansible/management.yml
```

Then enter in all the applications you want to install together.  In this example, we only want to install on the hosts in the group tests (10.36.36.32), but we could have easily enough replaced the group name “tests” with “all” and it would install these applications to all the hosts.  
```yml
--- # install or update all the apps I would like to have on mgmt hosts
- hosts: tests
  tasks:
  - name : install nmap
    yum:
      name: nmap
      state: latest
  - name: install My Traceroute
    yum:
      name: mtr
      state: latest
  - name: install screen
    yum:
      name: screen
      state: latest
  - name: install tcpdump
    yum:
      name: tcpdump
      state: latest
  - name: install dig
    yum:
      name: bind-utils
      state: latest
  - name: install nload
    yum:
      name: nload
      state: latest
  - name: install rsync
    yum:
      name: rsync
      state: latest
```

To review the format of the file above, we start with which hosts this will run against.  In this case, we’re looking to run this on all the “tests” the hosts
```yml
--- # install or update all the apps I would like to have on mgmt hosts
- hosts: tests
  tasks:
```

The module call first starts off with the name, explaining what is being done.  Then is the module call, in this example running a yum module.  We can format the arguments for yum either by listing them underneath the yum call like so
```yml
  - name : install nmap
    yum:
      name: nmap
      state: latest
```

Or you can put the arguments in a line after the module name, and separate them by spaces.
```yml
  - name : install nmap
    yum: name=nmap state=latest
```

Then we can simply run the playbook, and it will follow all of the instructions and get the work done. 
```bash
[root@boss01 ~]# ansible-playbook -i /root/ansible/hosts /root/ansible/management.yml

PLAY [tests] ********************************************************************************************

TASK [Gathering Facts] ****************************************************************************************
ok: [10.36.36.32]

TASK [install nmap] *******************************************************************************************
changed: [10.36.36.32]

TASK [install My Traceroute] **********************************************************************************
changed: [10.36.36.32]

TASK [install screen] *****************************************************************************************
ok: [10.36.36.32]

TASK [intall tcpdump] *****************************************************************************************
changed: [10.36.36.32]

TASK [install dig] ********************************************************************************************
ok: [10.36.36.32]

TASK [install nload] ******************************************************************************************
changed: [10.36.36.32]

TASK [install rsync] ******************************************************************************************
ok: [10.36.36.32]


PLAY RECAP ********************************************************************************************
10.36.36.32                : ok=8    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[root@boss01 ~]#
```


### Using Playbooks to Install, configure, and enable (including handlers)
But playbooks can do more than install or keep apps up to date.  You can also use them to enable services (make them active), and (as we learned above), we can even update their configs.  

In this example we will deploy ntp on a remote server, but to do this, we will need to have a config file for ntp, so we will create a directory, and put our ntp configuration into it.  
```bash
[root@boss01 ~]# mkdir /root/ansible/templates
[root@boss01 ~]# vim /root/ansible/templates/mb2-ntp.conf # and create the file
```

We can create the yaml file that will install ntp, configure it, and make sure that it’s running. 
```bash
[root@boss01 ~]# vim /root/ansible/ntp.yml
```

And the configuration file should look something like the following
```yml
--- # install, configure, and enable ntp
- hosts: all
  vars:
  tasks:

  - name: Set host timezone to UTC
    timezone:
      name: Zulu

  - name: Install NTP
    yum: name=ntp state=installed

  - name: copy the ntp config
    template: src=/root/ansible/templates/mb2-ntp.conf dest=/etc/ntp.conf
    notify:
      - restart: ntpd

  - name: shut down ntpd so we can do an initial sync, but also enable it after reboot
    service: name=ntpd state=stopped enabled=yes

  - name: Sync time initially
    shell: ntpdate 10.36.35.32

  - name: Bring NTP back up
    service: name=ntpd state=started enabled=yes

  - name: Sync hwclock to NTP time
    shell: hwclock -w

  handlers:
  - name: restart ntpd
    service: name=ntpd state=restarted
```

Going over the above:  In the beginning of the config above, we state that this will work with “all” the files in the hosts file.  Then we name each module with the name command (with a free text, unique name afterward), and then state the module (which I linked in this doc to their descriptions), and then include any attributes to that module.  


In the play “copy the ntp config”, we use the template module to move files, and if that succeeds, then it will run the “restart ntpd” handler.   A handler play is the same as a normal task, but that it will only run when it’s called (via a notify).  


When you run the module, the output will look like the following. 
```bash
[root@boss01 ~]# ansible-playbook -i /root/ansible/hosts /root/ansible/ntp.yml

PLAY [all] ****************************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [10.36.36.32]
ok: [10.33.128.150]

TASK [Set host timezone to UTC] *******************************************************
ok: [10.36.36.32]
ok: [10.33.128.150]

TASK [Install NTP] ********************************************************************
ok: [10.36.36.32]
ok: [10.33.128.150]

TASK [copy the ntp config] ************************************************************
ok: [10.36.36.32]
ok: [10.33.128.150]

TASK [shut down ntpd so we can do an initial sync] ************************************
ok: [10.36.36.32]
ok: [10.33.128.150]

TASK [Sync time initially] *************************************************************
changed: [10.36.36.32]
changed: [10.33.128.150]

TASK [Bring NTP back up] **************************************************************
changed: [10.33.128.150]
changed: [10.36.36.32]

TASK [Sync hwclock to NTP time] *******************************************************
changed: [10.33.128.150]
changed: [10.36.36.32]

PLAY RECAP ****************************************************************************
10.33.128.150              : ok=8    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
10.36.36.32                : ok=8    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[root@boss01 ~]#
```

## References 
- [Ansible Playbook Example – Sample Ansible Playbook](https://www.middlewareinventory.com/blog/ansible-playbook-example/): Admin, Mar 2020
- [Ansible Tutorial: Installation, Playbook, Roles, Commands](https://www.guru99.com/ansible-tutorial.html): Guru99, Feb 2020
- [An Ansible2 Tutorial](https://serversforhackers.com/c/an-ansible2-tutorial), Fideloper LLC, Feb, 2018
- [setting up ntp via Ansible in my private lab](https://mdschreier.com/2016/05/13/setting-up-ntp-via-ansible-in-my-private-lab/): Markus Schreier, May 2016
- [Playbook-ntp.yml](https://github.com/lastsky/ansible/blob/master/playbook-ntp.yml): lastsky, Dec 2016
- [A step by step guide to Ansible (Tutorial)](https://blog.ssdnodes.com/blog/step-by-step-ansible-guide/): Dwijadas Dey, Aug 2018
