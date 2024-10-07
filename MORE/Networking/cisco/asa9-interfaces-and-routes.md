# Interfaces and Routes

## Define VLANs

```
interface vlan 501
 des Comcast Internet
 nameif outside
 security-level 0
 ip address 50.195.1.189 255.255.255.248

interface vlan 10
 des internal network
 nameif inside
 security-level 70
 ip address 198.18.1.1 255.255.255.0
interface vlan 48
 des corp network
 nameif office
 security-level 71
 ip address 10.113.48.16 255.255.255.0
```
 

## Define Interfaces

```
int e0/0
 des Comcast Internet : swc1d01 [g1/0/4]
 switchport access vlan 501
 no shut
int e0/3
 des internal network : sw1 [g1/0/12]
 switchport access vlan 10
 no shut
int e0/2
 des office network : swc1d01 [g1/0/15]
 switchport access vlan 48
 no shut
```

## Define Routes

```
route outside 0 0 50.195.1.190 
route office 10.0.0.0 255.0.0.0 10.113.48.1
```