# Basic Physical Setup (v.11)

## Interfaces and routes:

### Mgmt interface
System :: Platform
```
- Management port conf = manual
- IP = 10.55.32.23
- Mask = 255.255.255.0
- Mgmt route = 10.55.32.1
- hostname = lb1n02.chuck.com
- host ip add = use management port address
- timezone = utc
- root password =
- admin password =
- ssh access = enabled
- ssh ip allow = all addresses
<update>
```

### create vlans, and assign to physical interfaces
Network :: VLANs :: <create>
```
- name = n1-trans1
- Description = front end vlan
- tag = 555
- interfaces: available = all
- interfaces: tagged = 1.1
<finished>
```

Network :: VLANs :: <create>
```
- name = n1-srv1
- Description = backend network
- tag = 550
- interfaces: available = all
- interfaces: tagged = 1.1
<finished>
```

### create IPs, and assign to VLANs
Network :: SelfIPs :: <create>

```
- name = n1-srv1
- IP address = 10.55.80.7
- Netmask = 255.255.255.0
- VLAN = n1-srv1
- port lockdown = allow none
- traffic group = traffic-group-local-only (non-floating)
<finished>
```

Network :: SelfIPs :: <create>
```
- name = n1-trans1
- IP address = 10.55.64.252
- Netmask = 255.255.255.0
- VLAN = n1-trans1
- port lockdown = allow all
- traffic group = traffic-group-local-only (non-floating)
<finished>
```

Network :: SelfIPs :: <create>
```
- name = n1-trans1_float
- IP address = 10.55.64.254
- Netmask = 255.255.255.0
- VLAN = n1-trans1
- traffic group = traffic-group-1 (floating)
<finished>
```

### define the default route:
Network :: Routes :: <add>
```
- name = Default
- description = general TMM outbound def routes
- destination = 0.0.0.0
- netmask = 0.0.0.0
- resource = use gateway
- gateway = ip address : 10.55.64.1
<finished>
```

## Management Settings:
### NTP:
System :: Configuration :: Device :: NTP
```
- address = 10.55.81.16
<add>
- address = 10.55.81.17
<add>
- address = 10.55.81.18
<add>
<update>
```

### DNS:
System :: Configuration :: Device :: DNS
```
DNS Lookup Server List
- Address = 10.55.81.17
<add>
- Address = 10.55.81.18
<add>
- Address = 10.120.81.17
<add>
DNS Search Domain List
- Address = chuck.com
<add>
<update>
```

### LDAP
System :: Users :: Authentication
```
<change>
- user directory = remote ldap
- host = 10.120.81.17
- port = 389
- remote directory tree = cn=users,cn=accounts,dc=chuck,dc=com
- scope = sub
- bind dn = f5_account
- bind pass = abc123
- user template = uid=f5_admin
```

### Syslog:
System :: Logs :: configuration :: Remote Logging
```
- remote IP = 10.55.81.20
- remote port = 514
<add>
- remote IP = 10.55.81.21
- remote port = 514
<add>
- remote IP = 10.120.81.20
- remote port = 514
<add>
<update>
```

## References:
- [Creating an Active-Standby Configuration using the Configuration Utility](https://support.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/bigip-device-service-clustering-admin-11-4-0/2.html):
- [BIG-IP Local Traffic Manager](https://support.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/ltm-concepts-11-4-0.html): Concepts
- [BIG-IP LTM 11.4.1 Documentation](https://support.f5.com/kb/en-us/products/big-ip_ltm/versions.11-4-1.html)
- [BIG-IP TMOS](http://support.f5.com/content/kb/en-us/products/big-ip_ltm/manuals/product/tmos-ip-routing-administration-11-3-0/_jcr_content/pdfAttach/download/file.res/BIG-IP_TMOS__IP_Routing_Administration.pdf): IP Routing Administration (v.11.3)
- [Configuring Routes](http://support.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/tmos_management_guide_10_1/tmos_routes.html): Chapter 10 (v10.1-10.2.4)


