# debug examples

## Example play with results 

Example play
```yml
---

- hosts: localhost
  gather_facts: yes
  tasks:
 
  - name: executing sample command
    shell: echo "Decoding DevOps"
    register: abcd
 
  - name: printing variable
    debug:
      var: abcd
 
  - name: printing variable with stdout
    debug:
      var: abcd.stdout
 
  - name: printing variable with adding some extra message
    debug: 
      msg: "Hi This Is  {{ abcd.stdout }}"

...
```
resulting output is this: 
```bash
cm@Balsa ~/techops/shared/cm/ansible $ ansible-playbook  playbooks/debug1.yml

PLAY [localhost] 
**************************************************************************************************

TASK [Gathering Facts]
********************************************************************************************
ok: [localhost]

TASK [executing sample command]
***********************************************************************************
changed: [localhost]

TASK [printing variable]
******************************************************************************************
ok: [localhost] => {
    "abcd": {
        "changed": true,
        "cmd": "echo \"Decoding DevOps\"",
        "delta": "0:00:00.007985",
        "end": "2020-05-15 12:31:56.116034",
        "failed": false,
        "rc": 0,
        "start": "2020-05-15 12:31:56.108049",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "Decoding DevOps",
        "stdout_lines": [
            "Decoding DevOps"
        ]
    }
}

TASK [printing variable with stdout] 
******************************************************************************
ok: [localhost] => {
    "abcd.stdout": "Decoding DevOps"
}

TASK [printing variable with adding some extra message] 
***********************************************************
ok: [localhost] => {
    "msg": "Hi This Is  Decoding DevOps"
}

PLAY RECAP 
********************************************************************************************************
localhost                  : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

cm@Balsa ~/techops/shared/cm/ansible $
```

## Going through all the steps

the shell module simply echo's the phrase "Decoding DevOps", and the register command takes the result of the shell module (the output "Decoding DevOps") and puts it into the variable "abcd".  
```yml
  - name: executing sample command
    shell: echo "Decoding DevOps"
    register: abcd
```

The next step outputs the value of var, and shows all the different pieces of data that is stored within it.  
```yml
  - name: printing variable
    debug:
      var: abcd
```

The output of this command is the following.  Notice that "stdout" is really what we are thinking of as the value of the variable, but there are many other components that we can use.  
```yml
TASK [printing variable] 
******************************************************************************************
ok: [localhost] => {
    "abcd": {
        "changed": true,
        "cmd": "echo \"Decoding DevOps\"",
        "delta": "0:00:00.007985",
        "end": "2020-05-15 12:31:56.116034",
        "failed": false,
        "rc": 0,
        "start": "2020-05-15 12:31:56.108049",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "Decoding DevOps",
        "stdout_lines": [
            "Decoding DevOps"
        ]
    }
}
```
If we want to just view the output of the "stdout" of the variable "abcd", then we refer to "abcd.stdout" like in the section below
```yml
  - name: printing variable with stdout
    debug:
      var: abcd.stdout
```

and get the result: 
```yml
TASK [printing variable with stdout]
******************************************************************************
ok: [localhost] => {
    "abcd.stdout": "Decoding DevOps"
}
```

we can also add text around the variable like the following by using the "{{ }}" brackets
```yml
  - name: printing variable with adding some extra message
    debug: 
      msg: "Hi This Is  {{ abcd.stdout }}"
```

Which will then produce the following: 
```yml
TASK [printing variable with adding some extra message] 
***********************************************************
ok: [localhost] => {
    "msg": "Hi This Is  Decoding DevOps"
}
```
