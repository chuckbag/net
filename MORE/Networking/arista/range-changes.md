# range changes


Make changes to multiple interfaces at once. 
In this example, we set ports 1 through 24 to be trunks and specify what vlans can talk to them.  This will paste the following lines to all of the interfaces in that range. 

```Shell
conf t
int e1-24
 switchport mode trunk
 switchport trunk allowed vlan 2,5,7,10-15
end
```



## References: 
- [EOS Tips for Power Users](https://eos.arista.com/introduction-to-managing-eos-devices-annex-b-eos-tips-for-power-users/): Arista, Oct 2014  
- [Command-Line Interface Commands](https://www.arista.com/en/um-eos/eos-section-3-10-command-line-interface-commands): Arista, 