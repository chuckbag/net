# confirming vxlans are seeing hosts


```Shell
nee-swc1c04#sh vxlan address-table
          Vxlan Mac Address Table
----------------------------------------------------------------------

Vlan  Mac Address     Type     Prt  Vtep             Moves   Last Move
----  -----------     ----     ---  ----             -----   ---------
  10  0000.0c9f.f00a  DYNAMIC  Vx1  10.153.255.3     1       1:38:30 ago
  10  002a.6aa2.7c20  DYNAMIC  Vx1  10.153.255.3     1       1:38:36 ago
  10  002a.6aa2.7c41  DYNAMIC  Vx1  10.153.255.3     1       1:38:31 ago
  10  002a.6aa2.7cc1  DYNAMIC  Vx1  10.153.255.3     1       1:18:25 ago
  10  0060.4827.5d02  DYNAMIC  Vx1  10.153.255.3     1       0:06:10 ago
  10  38c9.8637.48cf  DYNAMIC  Vx1  10.153.255.3     1       1:38:09 ago
  12  0000.0c9f.f00c  DYNAMIC  Vx1  10.153.255.3     1       1:38:30 ago
  12  002a.6aa2.7c20  DYNAMIC  Vx1  10.153.255.3     1       1:38:36 ago
  12  002a.6aa2.7c41  DYNAMIC  Vx1  10.153.255.3     1       1:38:30 ago
  12  002a.6aa2.7cc1  DYNAMIC  Vx1  10.153.255.3     1       0:13:21 ago
```

Prt = vxlan tunnel 1

Confirm that hosts directly connected to the switch are seen

```Shell
nee-swc1c04#sh mac address-table
          Mac Address Table
------------------------------------------------------------------

Vlan    Mac Address       Type        Ports      Moves   Last Move
----    -----------       ----        -----      -----   ---------
  10    0000.0c9f.f00a    DYNAMIC     Vx1        1       1:43:57 ago
  10    002a.6aa2.7c20    DYNAMIC     Vx1        1       1:44:04 ago
  10    002a.6aa2.7c41    DYNAMIC     Vx1        1       1:43:58 ago
  10    002a.6aa2.7cc1    DYNAMIC     Vx1        1       1:23:53 ago
  10    38c9.8637.48cf    DYNAMIC     Vx1        1       1:43:36 ago
  10    7426.ac5e.e883    DYNAMIC     Vx1        1       0:00:04 ago
  10    7426.ac5e.f783    DYNAMIC     Vx1        1       0:05:04 ago
  10    a036.9f28.e98a    DYNAMIC     Vx1        1       0:04:42 ago
  12    0000.0c9f.f00c    DYNAMIC     Vx1        1       1:43:57 ago
  12    0025.b5b0.001a    DYNAMIC     Vx1        1       0:04:43 ago
  12    0025.b5b0.001e    DYNAMIC     Vx1        1       0:04:40 ago
```
