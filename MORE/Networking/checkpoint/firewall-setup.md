# firewall setup

Checkpoint firewalls are broken up into two different components, the local firewall, and the management server.  The management server contains all the ACL's for all the firewalls, and pushes those configs to them.  The local firewall only contains interface, routes, and other administrative, (non-acl) related configs.  

## Switching between cli and shell: 
There are two different modes when you ssh into the firewall, the "clish" mode, and a bash prompt.  To switch between them use the exit and clish command.  
```
[Expert@sf-fw-01:0]# clish
sf-fw-01> 
sf-fw-01> exit
[Expert@sf-fw-01:0]# 
```
