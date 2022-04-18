# deleting entries

Delete, the opposite of the set command. 

## Add or delete a command: 
In this example, we add a new IP for an interface, and then remove the old one. 
```
set    interface ge-1/0/0 unit 0 family inet address 201.111.221.191/30
delete interface ge-1/0/0 unit 0 family inet address 32.142.12.162/30
```

## References: 
- [SRX Getting Started - CLI Modes and Features](http://kb.juniper.net/InfoCenter/index?page=content&id=KB15719): Juniper, Feb 14
