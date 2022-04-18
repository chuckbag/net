# Gentoo HowTo's

## Overview:

## Network:
## Changing the IP address:

Edit the file /etc/conf.d/net with the following changes:
```bash
vim /etc/conf.d/net
```

```
config_eth0="192.168.100.33 netmask 255.255.255.192 brd 192.168.100.255"
routes_eth0="default via 192.168.100.1"
dns_domain_lo="bos.corp"
```

Then restart the network interfaces to make the changes take effect.
```bash
/etc/init.d/net.eth0 restart
```

