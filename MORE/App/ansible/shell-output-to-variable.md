# shell output to variable

in this example, we can take the output of the date command, and accept it's results as a variable within ansible.  

The command module runs the shell command, and the registar argument then accepts the results and puts them in "command_output".  We then take the standard output of that and put it into the ansible variable "string_to_echo" which can then be used later in the ansible playbooks.
```yml
--- 
# playbooks/set-var.yml
#
# take the results of a shell command, and define it as a variable to be used later. 


- name: Demo
  hosts: localhost
  
 
  tasks:
  - name: Create variable from command
    command: "date +%y%m%d%H%M"
    register: command_output

  - set_fact:
      string_to_echo: "{{ command_output.stdout }}"

  - debug: 
      var: string_to_echo

#
...
```
The results of the play are the following: 
```bash
 $ ansible-playbook playbooks/set-var.yml


PLAY [Demo] ********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Create variable from command] ********************************************
changed: [localhost]

TASK [set_fact] ****************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "string_to_echo": "2006021656"
}

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

 $
```


## Ref: 
- [Ansible: Store command's stdout in new variable?](https://stackoverflow.com/questions/36059804/ansible-store-commands-stdout-in-new-variable): stackoverflow, March 2016
- [Ansible Command Module](https://docs.ansible.com/ansible/latest/modules/command_module.html): 
