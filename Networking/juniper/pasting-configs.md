# Pasting Configs

## Overview: 
You can paste your configs directly on the terminal, but things get a bit icky when you do that...

The contents
```
set system services dhcp pool 10.120.35.0/24 router 10.120.35.1
set system services dhcp pool 10.120.35.0/24 default-lease-time 3600

# 1.7 allow dhcp and ping to the firewall from the zone
set security zones security-zone mgmtCamera host-inbound-traffic system-services dhcp
set security zones security-zone mgmtCamera host-inbound-traffic system-services ping
```

Ends up getting messed up during a paste: 
```bash
[edit]
user@router# ...4 address-range high 10.120.35.159

[edit]
user@router# ...4 domain-name cmed.com

[edit]
user@router# ...p pool 10.120.35.0/24 name-server 8.8.4.4
```

There is a mode in JUNOS that is specially made for pasting configs.  You can paste both set commands and the C++ formatted code.

## Command: 
The command is `load merge terminal`, and to end it press `[control]+[d]` at the same time.
```
load merge terminal
! {paste your change} 
^D
```

### Special note: 
This sometimes goofs up when you are connecting though the RS232 port, as the buffers can get overrun.  

When your done, make sure that you check the results and that it's clean: 
```
commit check
show | compare
commit and-quit
```


## References: 
- [Uploading a Configuration File (CLI Procedure)](http://www.juniper.net/techpubs/en_US/release-independent/junos/topics/task/configuration/ex-series-configuration-uploading-cli.html): Sep 30, 2013
- ["Load merge terminal" is giving a syntax error with valid configuration](http://kb.juniper.net/InfoCenter/index?page=content&id=KB15472): kb15472, Nov 14, 2012