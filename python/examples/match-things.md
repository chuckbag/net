# Match Things



Input file

This is what the input file looks like: 
```
chuck@Balsa ~/bin/python-examples $ cat switch.conf
[...]
!
interface vlan 43
 name "mgmt-4 (noc)"
!
interface vlan 44
 name "mgmt5 (admin)"
!
interface TengigabitEthernet1/0/1
 description "fw1b01 [5] (cable3)"
 spanning-tree link-type point-to-point
 switchport mode trunk
 switchport trunk native vlan none
 macro description switch
 !next command is internal.
 macro auto smartport dynamic_type switch
!
interface TengigabitEthernet1/0/2
 description sga02
 switchport access vlan 10
!
interface TengigabitEthernet1/0/3
 switchport access vlan 10
!
interface TengigabitEthernet1/0/4
 switchport access vlan 10
!
interface TengigabitEthernet1/0/5
 switchport access vlan 10
!
interface TengigabitEthernet1/0/6
 switchport access vlan 10
!
interface TengigabitEthernet1/0/7
 description sga07
 switchport access vlan 10
!
interface TengigabitEthernet1/0/8
 description adb01
 switchport access vlan 10
!
interface TengigabitEthernet1/0/9
 description sga09
 switchport access vlan 10
!
interface TengigabitEthernet1/0/10
[...]
```

And we just want to match the port and the description.  

Code
```
#!/usr/bin/env python
#
# show examples of how to match 
import sys
import re

# Accept piped input
for line in sys.stdin:
    

    # find all lines with "TengigabitEthernet"
    if re.search("interface \w*TengigabitEthernet", line):
        physical = re.search("interface \w*TengigabitEthernet(.+)", line)
        phy = physical.group(1)
        phy = phy.strip('\n')
        phy = phy.strip('\r')

        print(" ")
        print("port: {}".format(phy))

    # find all lines that start with "description"
    if re.match("\s*description", line):
        description=re.search("\s*description\s+(.+)", line)
        des = description.group(1)
        des = des.strip('\n')
        des = des.strip('\r')
        des = des.replace(',', ' ')

        print("des: {}".format(des))
```

Output
```
chuck@Balsa ~/bin/python-examples $ cat switch.conf | ./matching-stuff.py

port: 1/0/1
des: "fw1b01 [5] (cable3)"

port: 1/0/2
des: sga02

port: 1/0/3

port: 1/0/4

port: 1/0/5

port: 1/0/6

port: 1/0/7
des: sga07

port: 1/0/8
des: adb01

port: 1/0/9
des: sga09

port: 1/0/10
des: sga10

port: 1/0/11
des: sga11

port: 1/0/12
des: "sw1p05 [47]"

port: 1/0/13
des: sga13

port: 1/0/14
des: "x11"

port: 1/0/15

port: 1/0/16

port: 1/0/17

port: 1/0/18
des: dga02

port: 1/0/19

port: 1/0/20

port: 1/0/21

port: 1/0/22

port: 1/0/23
des: sga12

port: 1/0/24
des: dga01
chuck@Balsa ~/bin/python-examples $
```

