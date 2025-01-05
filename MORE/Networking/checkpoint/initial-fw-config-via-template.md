# initial fw config via template

## Overview: 
You can configure a firewall via a template, or you can enter clish commands directly into the terminal to configure a box.  Here we review the method for setting up the box via a template. 

## Create the template file: 
Run the config_system command to create a template file that you can then later mark up.  To do this, run the command with a -t flag, and specify the path and filename to save the template to.  
```
[Expert@CKP-01:0]# config_system -t /home/admin/config_system.template
```

## Mark up the template file: 
```
#########################################################################
# #
# Products configuration #
# #
#    For keys below set "true"/"false" after '='  within the quotes #
#########################################################################
# 1.1 Install Security Gateway. 
install_security_gw=
# 1.2 Install Acceleration Blade (aka Performance Pack).
install_ppak=
# 1.3 Enable DAIP (dynamic ip) gateway.
# Should be "false" if CXL or Security Management enabled
gateway_daip="false"
# 1.4 Enable/Disable CXL.
gateway_cluster_member=
# 1.5 Install Security Management.
install_security_managment=
# 1.6 Optional parameters, only one of the parameters below can be "true".
# If no primary of secondary specified, log server will be installed.
# Requires Security Management to be installed.
install_mgmt_primary=
install_mgmt_secondary=
# 1.7 Provider-1 paramters
# e.g: install_mds_primary=true
#      install_mds_secondary=false
#      install_mlm=false
#      install_mds_interface=eth0
install_mds_primary=
install_mds_secondary=
install_mlm=
install_mds_interface=
# 1.8 Automatically download Blade Contracts and other important data (highly recommended)
# It is highly recommended to keep this setting enabled, to ensure smooth operation of Check Point products.
# for more info see sk94508 
#
# possible values: "true" / "false"
download_info="true"
# 1.9 Improve product experience by sending data to Check Point
# If you enable this setting, the Security Management Server and Security Gateways may upload data that will 
# help Check Point provide you with optimal services.
# for more info see sk94509
#
# possible values: "true" / "false"
upload_info="false"
# In case of Smart1 SmartEvent appliance, choose
# Security Management only, log server will be installed automatically
#########################################################################
# #
# Products Parameters #
# #
# For keys below set value after '=' #
#########################################################################
# 2.1 Management administrator name
# Must be provided, if Security Management installed
mgmt_admin_name=
# 2.2 Management administrator password
# Must be provided, if Security Management installed
mgmt_admin_passwd=
# 2.3 Management GUI client allowed e.g. any, 1.2.3.4, 192.168.0.0/24
# Set to "any" if any host allowed to connect to management
# Set to "range" if range of IPs allowed to connect to management
# Set to "network" if IPs from specific network allowed to connect 
# to management
# Set to "this" if it' a single IP
# Must be provided if Security Management installed
mgmt_gui_clients_radio=
# 
# 2.4 In case of "range", provide the first and last IPs in dotted format
mgmt_gui_clients_first_ip_field=
mgmt_gui_clients_last_ip_field=
#
# 2.5 In case of "network", provide IP in dotted format and netmask length 
# in range 0-32
mgmt_gui_clients_ip_field=
mgmt_gui_clients_subnet_field=
#
# 2.6 In case of a single IP
mgmt_gui_clients_hostname=
#
# 2.7 Secure Internal Communication key, e.g. "aaaa"
# Must be provided, if primary Security Management not installed
ftw_sic_key=
#########################################################################
# #
# Operating System configuration - optional section #
# #
# For keys below set value after '=' #
#########################################################################
# 3.1 Password (hash) of user admin.
# To get hash of admin password from configured system:  
# dbget passwd:admin:passwd
# OR
# grep admin /etc/shadow | cut -d: -f2
#
# IMPORTANT! In order to preserve the literal value of each character 
# in hash, inclose hash string within the quotes.
# e.g admin_hash='put_here_your_hash_string'
#
# Optional parameter
admin_hash=''
# 3.2 Interface name, optional parameter
iface=
# 3.3 Management interface IP in dotted format (e.g. 1.2.3.4),  
# management interface mask length (in range 0-32, e,g 24 ) and
# default gateway.
# Pay attention, that if you run first time configuration remotely 
# and you change IP, in order to maintain the connection, 
# an old IP address will be retained  as a secondary IP address. 
# This secondary IP address can be delete later. 
# Your session will be disconnected after first time configuration
# process.
# Optional parameter, requires "iface" to be specified
# IPv6 address format: 0000:1111:2222:3333:4444:5555:6666:7777
# ipstat_v4 manually/off
ipstat_v4=
ipaddr_v4=
masklen_v4=
default_gw_v4=
ipstat_v6=
ipaddr_v6=
masklen_v6=
default_gw_v6=
# 3.4 Host Name e.g host123, optional parameter
hostname=
# 3.5 Domain Name e.g. checkpoint.com, optional parameter 
domainname=
# 3.6 Time Zone in format Area/Region (e.g America/New_York or Etc/GMT-5)
# Pay attention that GMT offset should be in classic UTC notation:
# GMT-5 is 5 hours behind UTC (i.e. west to Greenwich)
# Inclose time zone string within the quotes.
# Optional parameter
timezone=''
# 3.7 NTP servers
# NTP parameters are optional
ntp_primary=
ntp_primary_version=
ntp_secondary=
ntp_secondary_version=
# 3.8 DNS - IP address of primary, secondary, tertiary DNS servers
# DNS parameters are optional.
primary=
secondary=
tertiary=
```

### Notes: 

Products Configuration: 
- Security Gateway: Make the box be a firewall. "Gateway: The security engine that enforces the organization’s security policy and acts as a security enforcement point."
- Acceleration Blade: 
- DAIP: 
- Gateway Cluster: Two firewalls in a cluster. 
- Security Management: Have the box be a manager, not a firewall (maybe you put both on?).  "Security Management Server: The server used by the system administrator to manage the security policy. The organization’s databases and security policies are stored on the Security Management Server and downloaded to the gateway."
- Redundant Security Management: enable redundant security management systems. 
- Provider-1 Parameters: 
- Auto Download updates: automatically grab updates from checkpoint.  (not os upgrades, but things like virus definitions.) 
- Stats to Checkpoint: Send debugging and use data to checkpoint

Products Parameters: 
- Admin Name: define admin account
- Admin Password: define password
- Enable GUI for Admin: 

## References: 
- [How to run the First Time Configuration Wizard through CLI in Gaia R76 and above](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk69701&partition=General&product=Security): Jan, 2017, sk69701