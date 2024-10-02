# reviewing vlans

see all the vlans, and what ports are associated

```Shell
nee-swc1c01#show vlan
VLAN  Name                             Status    Ports
----- -------------------------------- --------- -------------------------------
1     default                          active    Et7, Et8, Et25, Po1
10    mgmt                             active    Et25, Po1, Po2, Vx1
12    mgmt-ilo                         active    Et25, Po1, Po2, Vx1
15    srv-sales-eng                    active    Et25, Po1, Vx1
16    srv-2                            active    Et25, Po1, Vx1
999   DMZ1                             active    Et4, Et25, PEt4, Po1, Vx1
1001  b2-transit1                      active    Cpu, Et4, Et25, PEt4, Po1, Vx1
4092  iBGP_VRF_PUBLIC                  active    Cpu, Po1
4093  iBGP_VRF_DEFAULT                 active    Cpu, Po1
4094  mlagpeer                         active    Cpu, Po1
```

where `Et25` is the ethernet port 25, and `PEt4` is ethernet port 4 on the pair (other) switch.  

what interfaces are associated to a specific vlan

```Shell
nee-swc1c04#sh vlan id 51
VLAN  Name                             Status    Ports
----- -------------------------------- --------- -------------------------------
51    mgmt-video                       active    Cpu, Et1, Et2, Et3, Et4, Et5
                                                 Et6, Et7, Et8, Et9, Et10, Et13
                                                 Et14, Et15, Et16, Et17, Et18
                                                 Et19, Et20, Et21, Et22, Et23
                                                 Et24, Et45, Et46, Et47, Et48
                                                 PEt1, PEt2, PEt3, PEt4, PEt5
                                                 PEt6, PEt7, PEt8, PEt9, PEt10
                                                 PEt13, PEt14, PEt15, PEt16
                                                 PEt17, PEt18, PEt19, PEt20
                                                 PEt21, PEt22, PEt23, PEt24
                                                 PEt45, PEt46, PEt47, PEt48, Po1
                                                 Po2
```

looking at devices on that vlan: 

```Shell
nee-swc1c04#show ip arp int vl 51
Address         Age (min)  Hardware Addr   Interface
10.153.51.12          N/A  0010.f339.49c4  Vlan51, Port-Channel2
10.153.51.14          N/A  0050.56a9.0be6  Vlan51, Port-Channel1
```
