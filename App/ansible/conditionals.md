# Conditionals

- [Conditionals](#conditionals)
  - [Check global variable, and if match do something](#check-global-variable-and-if-match-do-something)
    - [Match a domain name](#match-a-domain-name)
    - [Match a collection of hosts](#match-a-collection-of-hosts)
  - [Reference:](#reference)

## Check global variable, and if match do something

### Match a domain name

The playbook first collects the contents of the "ansible_domain" variable, and the debug command prints out the domain.  Then it checks to see if that 

```yml
---
# collect some data from the host, depending on the data collected, do something. 

- hosts: localhost
  gather_facts: yes
  tasks: 

    - name: printing Variable
      debug: 
        var: ansible_domain
    
    - name: check if this is in mb2
      debug: 
        msg: ">> this is in MB2 "
      when: ansible_domain == "mb2.prod.cmed.us"
    
    - name: check if this is in fhm
      debug: 
        msg: ">> this is in FHM "
      when: ansible_domain == "fhm.prod.cmed.us"

...
```

If we run it on one test machine that does not match either of the domains, it skips the "check" task

```yml
cm@Balsa ~/ansible $ ansible-playbook  playbooks/condition-do.yml

PLAY [localhost] ************************************************************************************

TASK [Gathering Facts] ******************************************************************************
ok: [localhost]

TASK [printing Variable] ****************************************************************************
ok: [localhost] => {
    "ansible_domain": "0.0.127.in-addr.arpa"
}

TASK [check if this is in mb2] **********************************************************************
skipping: [localhost]

TASK [check if this is in fhm] **********************************************************************
skipping: [localhost]

PLAY RECAP ******************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

cm@Balsa ~/ansible $ 
```

If it runs on a host that does match, then it runs the task that matches.  
In this case, the domain output is "`mb2.prod.cmed.us`" which is matched in the second task.  With that match, the task prints out the message "`>> this is in MB2`", but you could modify that task so it would run a different module or even a role.  

```bash
[root@boss01 ansible]# ansible-playbook condition-do.yml

PLAY [localhost] ****************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************
ok: [localhost]

TASK [printing Variable] ********************************************************************************************************
ok: [localhost] => {
    "ansible_domain": "mb2.prod.cmed.us"
}

TASK [check if this is in mb2] **************************************************************************************************
ok: [localhost] => {
    "msg": ">> this is in MB2 "
}

TASK [check if this is in fhm] **************************************************************************************************
skipping: [localhost]

PLAY RECAP **********************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

[root@boss01 ansible]#
```

### Match a collection of hosts
If many hosts all share a similar prefix, you can grep them all out and do something with them.  In this example, we are going to search the hostname, and match on part of the name.  If there is a match, then we will do something.  

```yml
---
# only do something to hosts with common names

- hosts: all
  gather_facts: yes
  tasks: 

    - name: printing Variable
      debug: 
        var: ansible_hostname
    
    - name: check if this is a dga host
      debug: 
        msg: ">> this is a dga host "
      when: ansible_hostname is match("dga*")
...
```

An example of a match: 

```bash
# ansible-playbook playbooks/condition-hostgroup.yml -i 10.33.128.200, --user root --ask-pass
SSH password:

PLAY [all] ****************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [10.33.128.200]

TASK [printing Variable] **************************************************************************************************************************************
ok: [10.33.128.200] => {
    "ansible_hostname": "dga02"
}

TASK [check if this is a dga host] ****************************************************************************************************************************
ok: [10.33.128.200] => {
    "msg": ">> this is a dga host "
}

PLAY RECAP ****************************************************************************************************************************************************
10.33.128.200              : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[root@boss01 ansible]#
```

An example of a non-match: 

```bash
# ansible-playbook playbooks/condition-hostgroup.yml -i 10.33.128.150, --user root --ask-pass
SSH password:

PLAY [all] ****************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [10.33.128.150]

TASK [printing Variable] **************************************************************************************************************************************
ok: [10.33.128.150] => {
    "ansible_hostname": "test01"
}

TASK [check if this is a dga host] ****************************************************************************************************************************
skipping: [10.33.128.150]

PLAY RECAP ****************************************************************************************************************************************************
10.33.128.150              : ok=2    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

[root@boss01 ansible]#
```

## Reference: 
- [How to Use When Conditionals Statement in Ansible Playbook](https://www.linuxtechi.com/use-when-conditions-in-ansible-playbook/): Pradeep Kumar, March 2020
- [Where can I get a list of Ansible pre-defined variables?](https://stackoverflow.com/questions/18839509/where-can-i-get-a-list-of-ansible-pre-defined-variables): StackOverflow, Aug 2015
- [Using Variables: Ansible](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#magic-variables-and-how-to-access-information-about-other-hosts), v2.9
- [Conditionals](https://docs.ansible.com/ansible/2.3/playbooks_conditionals.html#id5): Ansible Core, v.2.3
- [Ansible conditionals - Wildcard match string](https://serverfault.com/questions/960575/ansible-conditionals-wildcard-match-string): Serverfault, Mar 2019
 


