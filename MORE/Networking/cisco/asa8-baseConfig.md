# ASA-Base Config

## Start Clean
- [Virginise the box](asa-virginise-the-box.md): how to clear the configs and bring the device back to the factory settings
- [Upgrade the OS](asa-upgrade-the-os.md): What steps to take to upgrade the firewall

## Base Configs:
- [Interfaces and Routes](asa8-interfaces-and-routes.md): How to define physical and virtual interfaces, and to setup basic static routes
- [User Accounts](asa8-user-accounts.md): How to configure users and allow them access to the firewall:
- [FIPS 140-2 Compliant](asa8-fips-140-2-compliant.md): A few additions to the config to make it a bit harder to crack
- [Failover Configuration](asa8-failover-configuration.md): What to do to pair two firewalls in an HA setup.
- [Syslogging](asa-syslogging.md): How to get your logs off the firewall, and how much to collect.
- [SNMP](snmp.md): How to allow snmp polling of your firewall. 
- [CLI Prompt](https://itsecworks.com/2010/11/12/cisco-asa-prompt-for-failover/): use the prompt command to change the prompt to indicate the status of the firewall
- [Upgrading the ASDM](asa-upgrading-the-asdm.md): There are a couple of things where you need the cisco gui. 
- [Adding new Certs](asa8-adding-new-certs.md): Steps to make add and move certs

## NAT/PATing:
- [Outbound PATing](asa-outbound-pating.md): (port address translation) How to set up many internal IP address in an office to all go out one IP on the outside of a firewall. 
- [Inbound NATing](asa-inbound-nating.md): (network address translation) How to send traffic to an internal server by using an IP address on the outside of the firewall. 

## Access Control Lists: 
- [Downward Flow](asa-downward-flow.md): The firewall has this idea of more and less secure networks.   Traveling "downward" (from a more secure to less) can be done without the need of an ACL.  
- [Interface ACL's](asa-interface-acls.md):  Reviews how you can create an ACL and bind it to an interface.

## References: 
- [FIPS 140-2 Security Policy for ASA 5500's](http://www.cisco.com/en/US/docs/security/asa/asa70/hw/fips_asa.html) (Cisco 2008)
- [ASA/PIX 7.x](http://www.cisco.com/en/US/products/hw/vpndevc/ps2030/products_configuration_example09186a00806e880b.shtml): Redundant or Backup ISP Links Configuration Example (Cisco 2008):
- [Cisco IOS IP SLAs Configuration Guide, Release 12.4 (Cisco )](http://www.cisco.com/en/US/docs/ios/12_4/ip_sla/configuration/guide/hsla_c.html): IOS and not for ASA, but can give you a better idea of all the things IP SLA can do.
- [Configuring Failover, v8.0 (cisco)](http://www.cisco.com/en/US/docs/security/asa/asa80/configuration/guide/failover.html): all the rules and theory for configuring fail-over on an ASA
- [Cisco ASA 5500 Migration to version 8.3, NAT Migration (cisco)](http://www.cisco.com/en/US/docs/security/asa/asa83/upgrading/migrating.html#wp83968): changes to the IOS Code
- [Cisco ASA 5500 Series Command Reference, 8.3](http://www.cisco.com/en/US/customer/docs/security/asa/asa83/command/reference/cmdref.html).  Details of all the commands
- [Cisco ASA 5500 Series Configuration Guide using the CLI, 8.3](http://www.cisco.com/en/US/docs/security/asa/asa83/configuration/guide/config.html): how to do the things to do.
- [Cisco ASA Version 8.3 NAT Configuration Example](http://www.youtube.com/watch?v=REGJodyLJEU): Good YOUTUBE video showing how natting and patting works (v8.3+) and how to debug.
