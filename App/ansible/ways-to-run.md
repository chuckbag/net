# Ways to Run


## Run a playbook

### With a defined group of hosts

if your in the ansible folder with the following directory structure: 
```
ansible/
├──+ dev/
│  └── hosts.txt
├──+ playbooks/
│  ├──  play1.yml
│  ├──  play2.yml
│  └──  play3.yml
└──+ roles/
   ├──  role1/
   ├──  role2/
   └──  role3/
```

Then when running a play from within the "ansible" directory, against the host list "hosts.txt" you would use the following command: 
```bash
$ ansible-playbook -i dev/hosts.txt playbooks/play1.yml
```

if the host.txt file was formatted with the following groups: 
```yml
[web]
web01.cmed.us
web02.cmed.us

[app]
app01.cmed.us
app02.cmed.us

[db]
db01.cmed.us
```

and you only wanted to run a play against the "[web]" hosts, you could run the play the following way: 
```bash
$ ansible-playbook -i dev/hosts.txt playbooks/play1.yml --limit web
```

### With a single host
You need to include the comma at the end so that ansible thinks you have a list of hosts.  
```bash
# ansible-playbook playbooks/condition-hostgroup.yml -i 10.33.128.200,
```


