# Tags

- [Tags](#tags)
  - [Playbooks:](#playbooks)
  - [Output:](#output)
    - [No tags:](#no-tags)
    - [tags = "t1"](#tags--t1)
    - [tags = "t2"](#tags--t2)
    - [tags = "t3"](#tags--t3)
  - [Ref:](#ref)

## Playbooks: 

playbook/tag-example.yml: 
```yml
---
- hosts: localhost
  roles: 
    - tag-example

...
```

roles/tag-example/tasks/main.yml: 
```yml
---
# tasks file for tag-example
- include: sub.yml 
  tags: t1

- debug: msg="> MAIN (T2)"
  tags: t2

- debug: msg="> MAIN (--no tag--)"
...
```

roles/tag-example/tasks/sub.yml: 
```yml
---
# tasks file for tag-example

- debug: msg=">> SUB (T1)"
  tags:
    - t1

- debug: msg=">> SUB (T2)"
  tags:
    - t2

- debug: msg=">> SUB (T3)"
  tags:
    - t3

- debug: msg=">> SUB (--no tag--)"
...
```

## Output: 

### No tags: 
Runs all the tasks, no matter what the tags are. 
```bash
$ ansible-playbook playbooks/tag-example.yml

PLAY [localhost] ***************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [localhost]

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T1)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T2)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T3)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (--no tag--)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": "> MAIN (T2)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": "> MAIN (--no tag--)"
}

PLAY RECAP *********************************************************************************************************
localhost                  : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### tags = "t1"
Plays everything in the sub play because it's tagged as "t1" in the main play.  (even though the tasks in sub are tagged differently.) 
Does not run the task in main that is not tagged 

```bash
$ ansible-playbook playbooks/tag-example.yml --tags "t1"

PLAY [localhost] ***************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [localhost]

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T1)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T2)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T3)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (--no tag--)"
}

PLAY RECAP *********************************************************************************************************
localhost                  : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### tags = "t2"
Plays everything in main AND sub that is tagged "t2" (it goes into the sub playbook, even though the main tags the playbook with a different tag.) Does not play untaged or tasks with different tags.  

```bash
$ ansible-playbook playbooks/tag-example.yml --tags "t2"

PLAY [localhost] ***************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [localhost]

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T2)"
}

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": "> MAIN (T2)"
}

PLAY RECAP *********************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### tags = "t3" 
Plays everything in sub that is tagged "t3" (it goes into the sub playbook, even though the main tags the playbook with a different tag.) Does not play untaged or tasks with different tags.
```bash
$ ansible-playbook playbooks/tag-example.yml --tags "t3"

PLAY [localhost] ***************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [localhost]

TASK [tag-example : debug] *****************************************************************************************
ok: [localhost] => {
    "msg": ">> SUB (T3)"
}

PLAY RECAP *********************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Ref: 
- [calling an ansible playbook with tag and parameter](https://stackoverflow.com/questions/40181416/calling-an-ansible-playbook-with-tag-and-parameter): Stack Overflow, Oct 2016