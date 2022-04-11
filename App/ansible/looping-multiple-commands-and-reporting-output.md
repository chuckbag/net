# Looping multiple commands, and reporting output

## The playbook: 
```yml
--- 
# playbooks/test.yml
#
# run a bunch of commands and view the results


- hosts: localhost
  gather_facts: false

  vars:
    numbers:
      - name: "current dir"
        cmd: "pwd"
      - name: "contents of dir"
        cmd: "ls"

  tasks:

    - name: Register output
      command: "{{ item.cmd }}"
      register: echo_out
      with_items: "{{ numbers }}"

    - name: view output
      debug: 
        msg: ">> {{item.item.name}} = {{item.stdout}}"
      with_items: "{{ echo_out.results }}"

# Ref: 
#  iteration using with_items and register: stackoverflow, June 2016
#  https://stackoverflow.com/questions/37925282/iteration-using-with-items-and-register
...
```

## The output: 
```bash
 $ ansible-playbook playbooks/multipleshellout.yml

PLAY [localhost] ********************************************************************************************

TASK [Register output] **************************************************************************************
changed: [localhost] => (item={'name': 'current dir', 'cmd': 'pwd'})
changed: [localhost] => (item={'name': 'contents of dir', 'cmd': 'ls'})

TASK [view output] ******************************************************************************************
ok: [localhost] => (item={'cmd': ['pwd'], 'stdout': '/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/playbooks', 'stderr': '', 'rc': 0, 'start': '2020-06-04 16:18:29.452670', 'end': '2020-06-04 16:18:29.458035', 'delta': '0:00:00.005365', 'changed': True, 'invocation': {'module_args': {'_raw_params': 'pwd', 'warn': True, '_uses_shell': False, 'stdin_add_newline': True, 'strip_empty_ends': True, 'argv': None, 'chdir': None, 'executable': None, 'creates': None, 'removes': None, 'stdin': None}}, 'stdout_lines': ['/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/playbooks'], 'stderr_lines': [], 'failed': False, 'item': {'name': 'current dir', 'cmd': 'pwd'}, 'ansible_loop_var': 'item'}) => {
    "msg": ">> current dir = /Users/cm/Documents/git/ansible/playbooks"
}
ok: [localhost] => (item={'cmd': ['ls'], 'stdout': 'bulkTemplates.yml\ncondition-do.yml\ndebug1.yml\nmultipleshellout.yml\nset-var.yml\ntag-example.yml\ntest.yml', 'stderr': '', 'rc': 0, 'start': '2020-06-04 16:18:29.759937', 'end': '2020-06-04 16:18:29.766749', 'delta': '0:00:00.006812', 'changed': True, 'invocation': {'module_args': {'_raw_params': 'ls', 'warn': True, '_uses_shell': False, 'stdin_add_newline': True, 'strip_empty_ends': True, 'argv': None, 'chdir': None, 'executable': None, 'creates': None, 'removes': None, 'stdin': None}}, 'stdout_lines': ['bulkTemplates.yml', 'condition-do.yml', 'debug1.yml', 'multipleshellout.yml', 'set-var.yml', 'tag-example.yml', 'test.yml'], 'stderr_lines': [], 'failed': False, 'item': {'name': 'contents of dir', 'cmd': 'ls'}, 'ansible_loop_var': 'item'}) => {
    "msg": ">> contents of dir = bulkTemplates.yml\ncondition-do.yml\ndebug1.yml\nmultipleshellout.yml\nset-var.yml\ntag-example.yml\ntest.yml"
}

PLAY RECAP **************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

 $
```

## Reference: 
- [iteration using with_items and register](https://stackoverflow.com/questions/37925282/iteration-using-with-items-and-register): stackoverflow, June 2016
