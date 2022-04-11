# moving folders of templates in roles
- [moving folders of templates in roles](#moving-folders-of-templates-in-roles)
  - [Overview](#overview)
  - [Code](#code)
    - [Overview of files:](#overview-of-files)
    - [Review of those files](#review-of-those-files)
      - [1. playbooks/bulkTemplates.yml file](#1-playbooksbulktemplatesyml-file)
      - [2. roles/bulkTemplates/tasks/main.yml file](#2-rolesbulktemplatestasksmainyml-file)
      - [3. roles/bulkTemplates/vars/main.yml file](#3-rolesbulktemplatesvarsmainyml-file)
      - [4. roles/bulkTemplates/templates/x1/file1.txt.j2 file](#4-rolesbulktemplatestemplatesx1file1txtj2-file)
  - [Results](#results)
  - [References:](#references)
  
## Overview
This example goes over a couple of items all in one.  The main goal is to demonstrate how to move folders of jinga files within your roles template directory.  To do this you need to include a regex to remove the .j2 file extension.  

## Code

### Overview of files: 
The directory structure we are dealing with, and what files are where is outlined below.  
ansible
```
 ├── playbooks
 |   └── bulkTemplates.yml           # (1)
 ├── roles
 |   └── bulkTemplates
 |       ├── tasks
 |       |   └── main.yml            # (2)
 |       ├── templates
 |       |   ├── x1 
 |       |   |   ├── file1.txt.j2    # (4)
 |       |   |   ├── file2.txt.j2    #
 |       |   |   └── file3.txt.j2    #
 |       |   └── x1 
 |       |       ├── file1.txt.j2    #
 |       |       ├── file2.txt.j2    #
 |       |       └── file3.txt.j2    #
 |       └── vars
 |           └── main.yml            # (3)
 └── testoutput                      # (5)
```

Ansible commands are run within the "ansible" directory.  
1. The main playbook that points to the role
2. the main task in the role that moves the files
3. the variables file used in the main task
4. the template files that are to be moved
5. the location where the template files are to be moved to

### Review of those files

#### 1. playbooks/bulkTemplates.yml file
This play defines a variable (that is not used because it is overwritten by the roles vars/main.yml value), and then runs the role. 
```yml
--- 
# playbooks/test.yml
#
# Run the test roles and see what it does


- name: Demo
  hosts: localhost
  gather_facts: False
  become: False

  vars: 
    - var1: "defined in the playbook"
  
  roles: 
    - bulkTemplates
      
...
```

#### 2. roles/bulkTemplates/tasks/main.yml file
First (1) we make sure that we have the correct directory structure where we want to put the resulting files.  

Then (2 & 3) we copy the files to the new location.  In this, there are a couple of things to note.  First is that the template command will look in the "roles/files" directory for the files to move.  Hence we need to first specify to go out a directory, then go into the templates and the following subdirectory to gather the files (`- ../templates/x1/*.j2`).  In the destination location, we specify the path we want it to use (`dest: ../testoutput/x1/`), but since we are using the files directories fully qualified current path (`{{ item }}`),  we need to first un-fully qualify it with the (`basename`) flag, and then remove the .j2 extension (`regex_replace('\.j2$', '')`) while we are putting it in the final location.  
```yml
---
# tasks file for bulkTemplates

# take files from the directories: 
#    roles/bulkTemplates/templates/x1/*
#    roles/bulkTemplates/templates/x2/*
# and put them in the directory test
# 
# BUT.... The files in the directory templates/x1 have .j2 extensions, 
# and we need to remove the extensions since they are in a subdirectory 
# within templates.  

- name: 1. make sure the testoutput directories exist
  file: 
    path: ../testoutput/{{ item }}
    state: directory
  loop: 
   - x1
   - x2

- name: 2. move directories of x1/*.j2 template files
  template: 
    src: "{{ item }}"
    dest: ../testoutput/x1/{{ item | basename |  regex_replace('\.j2$', '') }}
  with_fileglob:
    - ../templates/x1/*.j2

- name: 3. move directories of x2/*.j2 template files
  template: 
    src: "{{ item }}"
    dest: ../testoutput/x2/{{ item | basename |  regex_replace('\.j2$', '') }}
  with_fileglob:
    - ../templates/x2/*.j2

...
```

#### 3. roles/bulkTemplates/vars/main.yml file
This is the variable file that holds the variables for the role.  The roles defined in this file take precedent over files that are defined in the calling play. 
```yml
---
# vars file for bulkTemplates
var1: "defined in the bulkTemplates/vars/main.yml file"

...
```

#### 4. roles/bulkTemplates/templates/x1/file1.txt.j2 file
This is a Jinja file that includes variables within the {{ double_brackets}}.  When processed the variables will be replaced with the correct values, and the files will be copied to another location.  
```j2
# Start of text file
This is the start of the file

the variable is [ {{ var1 }} ]

This is the end of the file.
```

## Results
When you run the playbook, the command displays the following
```bash
 $ ansible-playbook playbooks/bulkTemplates.yml

PLAY [Demo] *********************************************************************************************************************************************************************************************************

TASK [bulkTemplates : make sure the testoutput directories exist] ***************************************************************************************************************************************************
changed: [localhost] => (item=x1)
changed: [localhost] => (item=x2)

TASK [bulkTemplates : move directories of x1/*.j2 template files] ***************************************************************************************************************************************************
changed: [localhost] => (item=/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/roles/bulkTemplates/files/../templates/x1/file2.txt.j2)
changed: [localhost] => (item=/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/roles/bulkTemplates/files/../templates/x1/file3.txt.j2)
changed: [localhost] => (item=/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/roles/bulkTemplates/files/../templates/x1/file1.txt.j2)

TASK [bulkTemplates : move directories of x2/*.j2 template files] ***************************************************************************************************************************************************
changed: [localhost] => (item=/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/roles/bulkTemplates/files/../templates/x2/file4.txt.j2)
changed: [localhost] => (item=/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/roles/bulkTemplates/files/../templates/x2/file6.txt.j2)
changed: [localhost] => (item=/Users/cmercier/Documents/git/techops/shared/cmercier/ansible/roles/bulkTemplates/files/../templates/x2/file5.txt.j2)

PLAY RECAP **********************************************************************************************************************************************************************************************************
localhost                  : ok=3    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

 $
```

And the jinja files (4) are processed and copied to the testoutput directory (6).  
```
ansible
 ├── playbooks
 |   └── bulkTemplates.yml           # (1)
 ├── roles
 |   └── bulkTemplates
 |       ├── tasks
 |       |   └── main.yml            # (2)
 |       ├── templates
 |       |   ├── x1 
 |       |   |   ├── file1.txt.j2    # (4)
 |       |   |   ├── file2.txt.j2    #
 |       |   |   └── file3.txt.j2    #
 |       |   └── x1 
 |       |       ├── file1.txt.j2    #
 |       |       ├── file2.txt.j2    #
 |       |       └── file3.txt.j2    #
 |       └── vars
 |           └── main.yml            # (3)
 └── testoutput                      # (5)
     ├── file1.txt                   # (6)
     ├── file2.txt                   # 
     ├── file3.txt                   # 
     ├── file4.txt                   # 
     ├── file5.txt                   # 
     └── file6.txt                   #
```

The resulting contents of "testoutput/file1.txt" includes the correct variable substitution.  
```
# Start of text file
This is the start of the file

the variable is [ defined in the bulkTemplates/vars/main.yml file ]

This is the end of the file.
```

## References: 
- [Deploying a folder of template files using ansible](https://serverfault.com/questions/578544/deploying-a-folder-of-template-files-using-ansible): Serverfault, Oct 2014