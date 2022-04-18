# Generic Redhat Notes

## Installing Apps with Yum:
see yum page for more details

- `yum -y check-update`
- `yum -y list installed` lists all apps installed on host
- `yum -y list` downloads header info of all apps available for install and stores it locally
- `yum -y check-update` upgrades all out of date apps.
- `yum search apache` search what is available for install with the name "apache"
- `yum install net-snmp-utils.x86_64` installs the app net-snmp-utils.x86_64

## Installing Apps with RPMs:
```bash
rpm -i -UVh <filename.rpm> Install a rpm 
```

## Starting and stopping services:
will do whatever to the iptables service: 
```bash
service iptables start|stop|status|restart 
```
