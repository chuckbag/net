# HA Setup

## HA
Ports for Syncing and Mirroring:
```
! - - - - - - - - - - - - - -
! login to primary:
https://10.55.32.22

Device Management :: Devices :: lb1n01.chuck.com (self) :: Device Connectivity :: ConfigSync

- local address = 10.55.64.253 (n1-trans1)
<update>

Device Management :: Devices :: lb1n01.chuck.com :: Device Connectivity :: Mirroring

- primary local mirror address = 10.55.64.253 (n1-trans1)
- secondary local mirror address = none
<update>
```

```
! - - - - - - - - - - - - - -
! login to secondary:
https://10.55.32.23

Device Management :: Devices :: lb1n02.chuck.com :: Device Connectivity :: ConfigSync

- local address = 10.55.64.252 (n1-trans1)
<update>

Device Management :: Devices :: lb1n02.chuck.com :: Device Connectivity :: Mirroring

- primary local mirror address = 10.55.64.252 (n1-trans1)
- secondary local mirror address = none
<update>
```

link the secondary to the primary
```
! - - - - - - - - - - - - - -
! login to primary:
https://10.55.32.22

Device Management :: Device Trust :: Peer List

<add>
- Device IP Address = 10.55.32.23
- Administrator Username = admin
- Administrator password =
<retrieve device information>

! confirm info is correct
<finished>
```

create the sync group and sync configs
```
Device Management :: Device Groups

<create>
- name = bos-nonpci-1
- group type = sync failover
- description = the boston non-pci loadbalance pair
- Members = lb1n01.prod.bos.credorax.com & lb1n02.prod.bos.credorax.com
- network failover = selected
- automatic sync = selected
- full sync = not-selected
<finished>
```

push the primary configs to the secondary
```
Device Management :: Overview :: Device Groups :: Name :: bos-nonpci-1

Devices :: lb1n01.chuck.com (self)

- sync device to group = selected
<sync>
```

### define IPs for failover comms:
```
! while still on the primary:

Device Management :: Devices :: lb1n01.chuck.com (self) :: device connectivity :: failover

<add>
- address = 10.55.64.253 (n1-trans1)
- port = 1026
<finished>

<add>
- address = 10.55.32.22 (management address)
- port = 1026
<finished>
```

```
! - - - - - - - - - - - - - -
! login to secondary:
https://10.55.32.22

Device Management :: Devices :: lb1n02.chuck.com (self) :: Device Connectivity :: Failover

<add>
- address = 10.55.64.252 (n1-trans1)
- port = 1026
<finish>

<add>
- address = 10.55.32.23 (management address)
- port = 1026
<finished>
```

### syncing:
```
! - - - - - - - - - - - - - -
! switch back to primary LB:
https://10.55.32.23

Device Management :: Overview :: Device Groups :: Name :: bos-nonpci-1

Devices :: lb1n01.chuck.com (self)
- sync device to group = selected
<sync>

! the end result should be one lb in "active" mode (green dot) and one in "standby" mode (gray dot)
```

### Manually fail back and forth.
```
! confirm that the primary is active:

Device Management :: Overview
! under "devices" the lb1n01.chuck.com (self) should be set to "active"
! flip to secondary:

Device Management :: Traffic Groups
- traffic-group-1 = selected

<force to standby>
<force to standby> ( a second time )
```

## References:
- [Creating an Active-Standby Configuration using the Configuration Utility](https://support.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/bigip-device-service-clustering-admin-11-4-0/2.html):
- [BIG-IP Local Traffic Manager](https://support.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/ltm-concepts-11-4-0.html): Concepts
- [BIG-IP LTM 11.4.1 Documentation](https://support.f5.com/kb/en-us/products/big-ip_ltm/versions.11-4-1.html)
- [Service port and protocol used for BIG-IP network failover](https://support.f5.com/kb/en-us/solutions/public/9000/000/sol9057.html)
- [Transport protocol used for BIG-IP connection and persistence mirroring](https://support.f5.com/kb/en-us/solutions/public/7000/200/sol7225.html)