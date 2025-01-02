# User Accounts
Some basic configs to get the box happy and healthy.

```
conf t
!
hostname fwgw1a
domain-name bos.corp
!
ntp server 18.7.63.43
enable password changeme!
!
aaa authentication serial console LOCAL
username <name> password <password>
aaa authentication ssh console LOCAL
aaa authentication enable console LOCAL
username <name> password <password> privilege 15
username <name> password <password> privilege 1
!
ssh 10.33.2.0 255.255.255.0 inside
ssh timeout 15
!
end
```

note that the ssh command allows you to ssh to the firewall from that network.

### Admin Only Accounts
This admin user can only connect to the CLI, and not VPN through the PIX

```
username user1 password w9Mi2asiKpn privilege 7
username user1 attributes
 service-type admin
```

### VPN Only Accounts
This user can IPSec VPN thorough the firewall, but can not connect to the CLI

```
username user2 password fyfzYqExIXMiWh3 privilege 1
username user2 attributes
 vpn-group-policy FULL_ACCESS
 vpn-filter value VPN_2_ALL
 vpn-tunnel-protocol ikev1
```

### Admin and VPN Accounts
This user can ssh directly to the CLI of the PIX, and also VPN through it's IPSec tunnel.

```
username user3 password fyfzYoTS0jXMiWh3 privilege 15
username user3 attributes
 service-type admin
 vpn-group-policy FULL_ACCESS
 vpn-filter value VPN_2_ALL
 vpn-tunnel-protocol IPSec
```

### Enabling SSH: 
Need to create the ca's : 

```
fw1(config)# crypto key generate rsa modulus 2048
INFO: The name for the keys will be: <Default-RSA-Key>
Keypair generation process begin. Please wait...
fw1(config)# 
```
