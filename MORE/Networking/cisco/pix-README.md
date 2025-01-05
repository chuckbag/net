# Older PIX documentation

This section contains info on understanding, setting up, and maintaining a Cisco PIX firewall.

- [Understanding the basics of a PIX firewall](pix-theory.md). A brief note on how to add hosts to the backside of a PIX, and how to make other minor changes to the PIX.
- [Basic Pix Config with notes](pix-config.md).  How to setup interfaces, and nat statements.
- [Example PIX Config with notes](pix-sample-config.md). Shows a basic net layout and the commands you would use to configure the PIX for that layout
- [Reset Password, and upgrade](pix-reset-pass.md): How to reset the password, and upgrade the OS at the same time
- [Upgrading HA PIXs](pix-failover.md): Steps to take to upgrade two pixes in HA
- [Enabling SSH and VPN Services](pix-asa-vpn-server.md). Steps to get the SSH daemon working on the pix, and also how to configure the pix to act as a vpn server.
- DHCP Server on a Pix: How to enable your firewall to also serve out IP addresses via DHCP.
- [Enabling a VPN between two Pixes](pix-to-pix-vpn.md). Writeup and examples for what you need to do to enable an ipsec vpn between PIX firewalls.

Sample Configs for Reference: 
- Fresh, out of the box [PIX config (5.3.1)](pix-outofbox_conf-5.3.1.md). This is for Cisco Secure PIX Firewall Version 5.3(1)
- Fresh, out of the box [PIX config (4.4.5)](pix-outofbox_conf-4.4.5.md). This is for Cisco Secure PIX Firewall Version 4.4(5)
