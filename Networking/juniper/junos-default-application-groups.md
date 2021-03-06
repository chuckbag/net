# Junos default application groups

The following document reviews all the `junos-*` applications: 

## viewing the ports for applications
```bash
fw> show configuration groups junos-defaults applications application junos-nfs
term t1 protocol udp destination-port 111;

fw> show configuration groups junos-defaults applications application junos-nfsd-udp
protocol udp;
destination-port 2049;

fw> show configuration groups junos-defaults applications application junos-nfsd-tcp
protocol tcp;
destination-port 2049;

fw>
```

## viewing all the possible pre-defined ports
You can view them directly from junos with the following command.  I've added the output here for easy review. 

```bash
> show configuration groups junos-defaults applications | no-more
#
# File Transfer Protocol
#
application junos-ftp {
    application-protocol ftp;
    protocol tcp;
    destination-port 21;
}
#
# Trivial File Transfer Protocol
#
application junos-tftp {
    application-protocol tftp;
    protocol udp;
    destination-port 69;
}
#
# Real Time Streaming Protocol
#
application junos-rtsp {
    application-protocol rtsp;
    protocol tcp;
    destination-port 554;
}
#
# Network Basic Input Output System  - networking protocol used on
# Windows networks   session service port
#
application junos-netbios-session {
    protocol tcp;
    destination-port 139;
}
application junos-smb-session {
    protocol tcp;
    destination-port 445;
}
application junos-ssh {
    protocol tcp;
    destination-port 22;
}
application junos-telnet {
    protocol tcp;
    destination-port 23;
}
application junos-smtp {
    protocol tcp;
    destination-port 25;
}
application junos-tacacs {
    protocol tcp;
    destination-port 49;
}
# TACACS Database Service
application junos-tacacs-ds {
    protocol tcp;
    destination-port 65;
}
application junos-dhcp-client {
    protocol udp;
    destination-port 68;
}
application junos-dhcp-server {
    protocol udp;
    destination-port 67;
}
application junos-bootpc {
    protocol udp;
    destination-port 68;
}
application junos-bootps {
    protocol udp;
    destination-port 67;
}
application junos-finger {
    protocol tcp;
    destination-port 79;
}
application junos-http {
    application-protocol http;
    protocol tcp;
    destination-port 80;
}
application junos-https {
    protocol tcp;
    destination-port 443;
}
application junos-pop3 {
    protocol tcp;
    destination-port 110;
}
application junos-ident {
    protocol tcp;
    destination-port 113;
}
application junos-nntp {
    protocol tcp;
    destination-port 119;
}
application junos-ntp {
    protocol udp;
    destination-port 123;
}
application junos-imap {
    protocol tcp;
    destination-port 143;
}
application junos-imaps {
    protocol tcp;
    destination-port 993;
}
application junos-bgp {
    protocol tcp;
    destination-port 179;
}
application junos-ldap {
    protocol tcp;
    destination-port 389;
}
application junos-snpp {
    protocol tcp;
    destination-port 444;
}
application junos-biff {
    protocol udp;
    destination-port 512;
}
# UNIX who
application junos-who {
    protocol udp;
    destination-port 513;
}
application junos-syslog {
    protocol udp;
    destination-port 514;
}
# line printer daemon, printer, spooler
application junos-printer {
    protocol tcp;
    destination-port 515;
}
application junos-rip {
    protocol udp;
    destination-port 520;
}
# INA sanctioned RADIUS port numbers
application junos-radius {
    protocol udp;
    destination-port 1812;
}
application junos-radacct {
    protocol udp;
    destination-port 1813;
}
application junos-nfsd-tcp {
    protocol tcp;
    destination-port 2049;
}
application junos-nfsd-udp {
    protocol udp;
    destination-port 2049;
}
application junos-cvspserver {
    protocol tcp;
    destination-port 2401;
}
#
# Label Distribution Protocol
#
application junos-ldp-tcp {
    protocol tcp;
    destination-port 646;
}
application junos-ldp-udp {
    protocol udp;
    destination-port 646;
}
#
# JUNOScript and JUNOScope management
#
application junos-xnm-ssl {
    protocol tcp;
    destination-port 3220;
}
application junos-xnm-clear-text {
    protocol tcp;
    destination-port 3221;
}
#
# IPSec tunnel
#
application junos-ike {
    protocol udp;
    destination-port 500;
}
#
# Any IPv4 application
#
application any {
    term t1 protocol 0;
}
#
# America Online instant messaging services
#
application junos-aol {
    term t1 protocol 6 destination-port 5190-5193;
}
#
# Character generator protocol
#
application junos-chargen {
    term t1 protocol udp destination-port 19;
}
#
# DHCP Relay services
#
application junos-dhcp-relay {
    term t1 protocol udp destination-port 67;
}
#
# Discard  protocol
#
application junos-discard {
    term t1 protocol udp destination-port 9;
}
#
# DNS
#
application junos-dns-udp {
    term t1 alg dns protocol udp destination-port 53;
}
application junos-dns-tcp {
    term t1 alg dns protocol tcp destination-port 53;
}
#
# Echo protocol
#
application junos-echo {
    term t1 protocol udp destination-port 7;
}
#
# Gopher internet protocol
#
application junos-gopher {
    term t1 protocol tcp destination-port 70;
}
#
# GPRS Tunneling Protocol UDP
#
application junos-gtp {
    term t1 protocol udp destination-port 2123;
}
#
# Gnutella File Sharing Protocol
#
application junos-gnutella {
    term t1 protocol udp destination-port 6346-6347;
}
#
# Generic Routing Encapsulation Protocol
#
application junos-gre {
    term t1 protocol 47;
}
#
# HTTP extension
#
application junos-http-ext {
    term t1 protocol tcp destination-port 7001;
}
#
# ICMP All Traffic
#   This can be made to be more restrictive by specifying icmp
#   type and code.
#
application junos-icmp-all {
    term t1 protocol icmp;
}
#
# ICMP Ping.
#   The echo-reply is allowed upon return
#
application junos-icmp-ping {
    term t1 protocol icmp icmp-type echo-request;
}
#
# Internet locator service
#
application junos-internet-locator-service {
    term t1 protocol tcp destination-port 389;
}
#
# IKE protocol
#
application junos-ike-nat {
    term t1 protocol udp destination-port 4500;
}
#
# Internet Relay Chat protocol
#
application junos-irc {
    term t1 protocol tcp destination-port 6660-6669;
}
#
# L2TP tunnelng protocol
#
application junos-l2tp {
    term t1 protocol udp destination-port 1701;
}
#
# Line Printer Daemon protocol
#
application junos-lpr {
    term t1 protocol tcp destination-port 515;
}
#
# Mail (SMTP) protocol
#
application junos-mail {
    term t1 protocol tcp destination-port 25;
}
#
# H.323 Protocol for audio/video conferencing
#
application junos-h323 {
    term t1 alg q931 protocol tcp destination-port 1720;
    term t2 alg ras protocol udp destination-port 1719;
    term t3 protocol tcp destination-port 1503;
    term t4 protocol tcp destination-port 389;
    term t5 protocol tcp destination-port 522;
    term t6 protocol tcp destination-port 1731;
}
#
# MGCP Protocol
#
application junos-mgcp-ua {
    term t1 alg mgcp-ua protocol udp destination-port 2427;
}
application junos-mgcp-ca {
    term t1 alg mgcp-ca protocol udp destination-port 2727;
}
#
# Microsoft Network Messenger
#
application junos-msn {
    term t1 protocol tcp destination-port 1863;
}
#
#  Microsoft RPC
#
application junos-ms-rpc-tcp {
    term t1 alg ms-rpc protocol tcp destination-port 135;
}
application junos-ms-rpc-udp {
    term t1 alg ms-rpc protocol udp destination-port 135;
}
#
#  Microsoft RPC EPM (End Point Mapper)
#
application junos-ms-rpc-epm {
    term t1 protocol tcp uuid e1af8308-5d1f-11c9-91a4-08002b14a0fa;
}
#
#  Microsoft RPC Exchange Directory RFR
#
application junos-ms-rpc-msexchange-directory-rfr {
    term t1 protocol tcp uuid 1544f5e0-613c-11d1-93df-00c04fd7bd09;
}
#
#  Microsoft RPC Exchange Information Store
#
application junos-ms-rpc-msexchange-info-store {
    term t1 protocol tcp uuid a4f1db00-ca47-1067-b31f-00dd010662da;
}
#
#  Microsoft RPC Exchange Directory NSP
#
application junos-ms-rpc-msexchange-directory-nsp {
    term t1 protocol tcp uuid f5cc5a18-4264-101a-8c59-08002b2f8426;
}
#
#  Microsoft RPC DCOM
#
application junos-ms-rpc-wmic-admin {
    term t1 protocol tcp uuid a9e69610-b80d-11d0-b9b9-00a0c922e750;
}
application junos-ms-rpc-wmic-webm-level1login {
    term t1 protocol tcp uuid f309ad18-d86a-11d0-a075-00c04fb68820;
}
application junos-ms-rpc-wmic-admin2 {
    term t1 protocol tcp uuid 29822ab8-f302-11d0-9953-00c04fd919c1;
}
application junos-ms-rpc-wmic-mgmt {
    term t1 protocol tcp uuid 8bc3f05e-d86b-11d0-a075-00c04fb68820;
}
application junos-ms-rpc-iis-com-1 {
    term t1 protocol tcp uuid a9e69612-b80d-11d0-b9b9-00a0c922e750;
}
application junos-ms-rpc-iis-com-adminbase {
    term t1 protocol tcp uuid 70b51430-b6ca-11d0-b9b9-00a0c922e750;
}
#
# MS RPC any
#
application junos-ms-rpc-uuid-any-tcp {
    term t1 protocol tcp uuid ffffffff-ffff-ffff-ffff-ffffffffffff;
}
application junos-ms-rpc-uuid-any-udp {
    term t1 protocol udp uuid ffffffff-ffff-ffff-ffff-ffffffffffff;
}
#
#  Microsoft SQL
#
application junos-ms-sql {
    term t1 protocol tcp destination-port 1433;
}
#
# NetBIOS Name Service
#
application junos-nbname {
    term t1 protocol udp destination-port 137;
}
#
# NetBIOS Datagram Service
#
application junos-nbds {
    term t1 protocol udp destination-port 138;
}
#
# Network File System  protocol
#
application junos-nfs {
    term t1 protocol udp destination-port 111;
}
#
# NS-Global (Management protocol for Juniper Networks Firewall/VPN devices)
#
application junos-ns-global {
    term t1 protocol tcp destination-port 15397;
}
#
# NS-Global-PRO (Monitoring system for the Juniper Networks Firewall/VPN devices)
#
application junos-ns-global-pro {
    term t1 protocol tcp destination-port 15397;
}
#
# NetScreen Security Manager
#
application junos-nsm {
    term t1 protocol udp destination-port 69;
}
#
# OSPF protocol
#
application junos-ospf {
    term t1 protocol 89;
}
#
# PC-anywhere remote control and file transfer protocol
#
application junos-pc-anywhere {
    term t1 protocol udp destination-port 5632;
}
#
# Ping  protocol
#
application junos-ping {
    term t1 protocol 1;
}
#
# Ping for IPv6
#
application junos-pingv6 {
    term t1 protocol 58;
}
#
# ICMP6 destination unreachable address
#
application junos-icmp6-dst-unreach-addr {
    term t1 protocol 58 icmp6-type 1 icmp6-code 3;
}
#
# ICMP6 destination unreachable administration
#
application junos-icmp6-dst-unreach-admin {
    term t1 protocol 58 icmp6-type 1 icmp6-code 1;
}
#
# ICMP6 destination unreachable beyond
#
application junos-icmp6-dst-unreach-beyond {
    term t1 protocol 58 icmp6-type 1 icmp6-code 2;
}
#
# ICMP6 destination unreachable port
#
application junos-icmp6-dst-unreach-port {
    term t1 protocol 58 icmp6-type 1 icmp6-code 4;
}
#
# ICMP6 destination unreachable route
#
application junos-icmp6-dst-unreach-route {
    term t1 protocol 58 icmp6-type 1 icmp6-code 0;
}
#
# ICMP6 echo reply
#
application junos-icmp6-echo-reply {
    term t1 protocol 58 icmp6-type 129;
}
#
# ICMP6 echo request
#
application junos-icmp6-echo-request {
    term t1 protocol 58 icmp6-type 128;
}
#
# ICMP6 packet too big
#
application junos-icmp6-packet-to-big {
    term t1 protocol 58 icmp6-type 2 icmp6-code 0;
}
#
# ICMP6 parameter problem header
#
application junos-icmp6-param-prob-header {
    term t1 protocol 58 icmp6-type 4 icmp6-code 0;
}
#
# ICMP6 parameter problem next header
#
application junos-icmp6-param-prob-nexthdr {
    term t1 protocol 58 icmp6-type 4 icmp6-code 1;
}
#
# ICMP6 parameter problem option
#
application junos-icmp6-param-prob-option {
    term t1 protocol 58 icmp6-type 4 icmp6-code 2;
}
#
# ICMP6 time exceeded reassembly
#
application junos-icmp6-time-exceed-reassembly {
    term t1 protocol 58 icmp6-type 3 icmp6-code 1;
}
#
# ICMP6 time exceeded transit
#
application junos-icmp6-time-exceed-transit {
    term t1 protocol 58 icmp6-type 3 icmp6-code 0;
}
#
# ICMP6 all traffic
#   This can be made to be more restrictive by specifying icmp6
#   type and code.
#
application junos-icmp6-all {
    term t1 protocol 58;
}
#
# Point-to-Point Tunneling protocol
#
application junos-pptp {
    term t1 alg pptp protocol tcp destination-port 1723;
}
#
# Real players use this protocol for real time streaming
# This was the original protocol for real players.
# RTSP is more widely used by real players
# but they still support realaudio.
#
application junos-realaudio {
    term t1 alg rtsp protocol tcp destination-port 554;
}
#
# Cisco Station Call Control Protocol
#
application junos-sccp {
    term t1 alg sccp protocol tcp destination-port 2000;
}
application junos-sctp-any {
    term t1 protocol 132;
}
#
# Session Initiation Protocol (SIP)
#
application junos-sip {
    term t1 alg sip protocol udp destination-port 5060;
    term t2 alg sip protocol tcp destination-port 5060;
}
#
# RSH
#
application junos-rsh {
    term t1 alg rsh protocol tcp destination-port 514;
}
#
# Server Message Block Protocol
#
application junos-smb {
    term t1 protocol tcp destination-port 139;
    term t2 protocol tcp destination-port 445;
}
application junos-sql-monitor {
    term t1 protocol udp destination-port 1434;
}
#
# Oracle SQL*Net Version 1
#
application junos-sqlnet-v1 {
    term t1 protocol tcp destination-port 1525;
}
#
# Oracle SQL*Net Version 2
#
application junos-sqlnet-v2 {
    term t1 alg sqlnet-v2 protocol tcp destination-port 1521;
}
#
# Sun RPC
#
application junos-sun-rpc-tcp {
    term t1 alg sun-rpc protocol tcp destination-port 111;
}
application junos-sun-rpc-udp {
    term t1 alg sun-rpc protocol udp destination-port 111;
}
#
# Sun RPC Portmapper
#
application junos-sun-rpc-portmap-tcp {
    term t1 protocol tcp rpc-program-number 100000;
}
application junos-sun-rpc-portmap-udp {
    term t1 protocol udp rpc-program-number 100000;
}
#
# Sun RPC nfs
#
application junos-sun-rpc-nfs-tcp {
    term t1 protocol tcp rpc-program-number 100003;
}
application junos-sun-rpc-nfs-udp {
    term t1 protocol udp rpc-program-number 100003;
}
#
# Sun RPC mountd
#
application junos-sun-rpc-mountd-tcp {
    term t1 protocol tcp rpc-program-number 100005;
}
application junos-sun-rpc-mountd-udp {
    term t1 protocol udp rpc-program-number 100005;
}
#
# Sun RPC ypbind
#
application junos-sun-rpc-ypbind-tcp {
    term t1 protocol tcp rpc-program-number 100007;
}
application junos-sun-rpc-ypbind-udp {
    term t1 protocol udp rpc-program-number 100007;
}
#
# Sun RPC status
#
application junos-sun-rpc-status-tcp {
    term t1 protocol tcp rpc-program-number 100024;
}
application junos-sun-rpc-status-udp {
    term t1 protocol udp rpc-program-number 100024;
}
#
# Sun RPC ypserv
#
application junos-sun-rpc-ypserv-tcp {
    term t1 protocol tcp rpc-program-number 100004;
}
application junos-sun-rpc-ypserv-udp {
    term t1 protocol udp rpc-program-number 100004;
}
#
# Sun RPC Remote Quota Daemon
#
application junos-sun-rpc-rquotad-tcp {
    term t1 protocol tcp rpc-program-number 100011;
}
application junos-sun-rpc-rquotad-udp {
    term t1 protocol udp rpc-program-number 100011;
}
#
# Sun RPC Network Lock Manager
#
application junos-sun-rpc-nlockmgr-tcp {
    term t1 protocol tcp rpc-program-number 100021;
}
application junos-sun-rpc-nlockmgr-udp {
    term t1 protocol udp rpc-program-number 100021;
}
#
# Sun RPC Remote User Daemon
#
application junos-sun-rpc-ruserd-tcp {
    term t1 protocol tcp rpc-program-number 100002;
}
application junos-sun-rpc-ruserd-udp {
    term t1 protocol udp rpc-program-number 100002;
}
#
# Sun RPC System Administration Daemon
#
application junos-sun-rpc-sadmind-tcp {
    term t1 protocol tcp rpc-program-number 100232;
}
application junos-sun-rpc-sadmind-udp {
    term t1 protocol udp rpc-program-number 100232;
}
#
# Sun RPC SPRAY Daemon
#
application junos-sun-rpc-sprayd-tcp {
    term t1 protocol tcp rpc-program-number 100012;
}
application junos-sun-rpc-sprayd-udp {
    term t1 protocol udp rpc-program-number 100012;
}
#
# Sun RPC WALL Daemon
#
application junos-sun-rpc-walld-tcp {
    term t1 protocol tcp rpc-program-number 100008;
}
application junos-sun-rpc-walld-udp {
    term t1 protocol udp rpc-program-number 100008;
}
#
# SUN RPC any
#
application junos-sun-rpc-any-tcp {
    term t1 protocol tcp rpc-program-number 1610612735;
}
application junos-sun-rpc-any-udp {
    term t1 protocol udp rpc-program-number 1610612735;
}
# UNIX talk
application junos-talk {
    term t1 alg talk protocol udp destination-port 517;
    term t2 alg talk protocol tcp destination-port 517;
}
application junos-ntalk {
    term t1 alg talk protocol udp destination-port 518;
    term t2 alg talk protocol tcp destination-port 518;
}
#
# Radio-Router Control Protocol
#
application junos-r2cp {
    term t1 protocol udp destination-port 28672;
}
#
# Any TCP application
#
application junos-tcp-any {
    term t1 protocol tcp;
}
#
# Any UDP application
#
application junos-udp-any {
    term t1 protocol udp;
}
#
# Unix to Unix Copy (UUCP) Protocol
#
application junos-uucp {
    term t1 protocol udp destination-port 540;
}
#
# VDOLive video streaming technology
#
application junos-vdo-live {
    term t1 protocol udp destination-port 7000-7010;
}
#
# Virtual Network Computing's protocol
#
application junos-vnc {
    term t1 protocol tcp destination-port 5800;
}
#
# Wide Area Information Server
#
application junos-wais {
    term t1 protocol tcp destination-port 210;
}
#
# Network Directory Service Protocol
#
application junos-whois {
    term t1 protocol tcp destination-port 43;
}
#
# WinFrame protocol (allows users on non-windows machines to run windows applications)
#
application junos-winframe {
    term t1 protocol tcp destination-port 1494;
}
#
# X-Windows         protocol
#
application junos-x-windows {
    term t1 protocol tcp destination-port 6000-6063;
}
#
# Yahoo Messenger
#
application junos-ymsg {
    term t1 protocol tcp destination-port 5000-5010;
    term t2 protocol tcp destination-port 5050;
    term t3 protocol udp destination-port 5000-5010;
    term t4 protocol udp destination-port 5050;
}
#
# WX Control Connection used by WX-PIM
#
application junos-wxcontrol {
    term t1 protocol tcp destination-port 3578 inactivity-timeout 7560;
}
#
# SNMP AgentX Connection used by WX-ISM
#
application junos-snmp-agentx {
    term t1 protocol tcp destination-port 705 inactivity-timeout 7560;
}
#
# Simple Traversal of User Datagram Protocol (UDP) Through
# Network Address Translators (NATs)
#
application junos-stun {
    term t1 protocol udp destination-port 3478-3479;
    term t2 protocol tcp destination-port 3478-3479;
}
#
# Persistent NAT Service
#
application junos-persistent-nat {
    term t1 protocol 255 source-port 65535 destination-port 65535;
}
#
# 'junos-routing-inbound' represents routing protocols that may
# that may need access the trusted network from the untrusted
# network.
#
# Set is intended for a UI to display routing involvement choices.
#
# NOTE:  It is not recommended you use the entire set directly in
#        a firewall rule and open up firewall to all of these
#        applications.  Also, you should always specify the source
#        and destination prefixes when using each application.
#
# NOTE: the contents of this set may grow in future JUNOS versions.
#
application-set junos-routing-inbound {
    application junos-bgp;
    application junos-rip;
    application junos-ldp-tcp;
    application junos-ldp-udp;
}
#
# Common Internet File System (cifs)
# It runs over netbios and over smb, so construct
# an application set to capture it.
#
application-set junos-cifs {
    application junos-netbios-session;
    application junos-smb-session;
}
application-set junos-mgcp {
    application junos-mgcp-ua;
    application junos-mgcp-ca;
}
application-set junos-ms-rpc {
    application junos-ms-rpc-tcp;
    application junos-ms-rpc-udp;
}
#
#  Microsoft RPC Microsoft Exchange
#
application-set junos-ms-rpc-msexchange {
    application junos-ms-rpc-tcp;
    application junos-ms-rpc-udp;
    application junos-ms-rpc-epm;
    application junos-ms-rpc-msexchange-directory-rfr;
    application junos-ms-rpc-msexchange-info-store;
    application junos-ms-rpc-msexchange-directory-nsp;
}
application-set junos-ms-rpc-wmic {
    application junos-ms-rpc-tcp;
    application junos-ms-rpc-wmic-admin;
    application junos-ms-rpc-wmic-admin2;
    application junos-ms-rpc-wmic-webm-level1login;
    application junos-ms-rpc-wmic-mgmt;
}
application-set junos-ms-rpc-iis-com {
    application junos-ms-rpc-tcp;
    application junos-ms-rpc-iis-com-1;
    application junos-ms-rpc-iis-com-adminbase;
}
application-set junos-ms-rpc-any {
    application junos-ms-rpc-tcp;
    application junos-ms-rpc-udp;
    application junos-ms-rpc-uuid-any-tcp;
    application junos-ms-rpc-uuid-any-udp;
}
application-set junos-sun-rpc {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
}
application-set junos-sun-rpc-portmap {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
}
application-set junos-sun-rpc-nfs {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-nfs-tcp;
    application junos-sun-rpc-nfs-udp;
}
application-set junos-sun-rpc-mountd {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-mountd-tcp;
    application junos-sun-rpc-mountd-udp;
}
application-set junos-sun-rpc-ypbind {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-ypbind-tcp;
    application junos-sun-rpc-ypbind-udp;
}
application-set junos-sun-rpc-status {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-status-tcp;
    application junos-sun-rpc-status-udp;
}
#
# Sun RPC nfs-access (Requires nfs and mountd)
#
application-set junos-sun-rpc-nfs-access {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-nfs-tcp;
    application junos-sun-rpc-nfs-udp;
    application junos-sun-rpc-mountd-tcp;
    application junos-sun-rpc-mountd-udp;
}
application-set junos-sun-rpc-ypserv {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-ypserv-tcp;
    application junos-sun-rpc-ypserv-udp;
}
application-set junos-sun-rpc-rquotad {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-rquotad-tcp;
    application junos-sun-rpc-rquotad-udp;
}
application-set junos-sun-rpc-nlockmgr {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-nlockmgr-tcp;
    application junos-sun-rpc-nlockmgr-udp;
}
application-set junos-sun-rpc-ruserd {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-ruserd-tcp;
    application junos-sun-rpc-ruserd-udp;
}
application-set junos-sun-rpc-sadmind {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-sadmind-tcp;
    application junos-sun-rpc-sadmind-udp;
}
application-set junos-sun-rpc-sprayd {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-sprayd-tcp;
    application junos-sun-rpc-sprayd-udp;
}
application-set junos-sun-rpc-walld {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-portmap-tcp;
    application junos-sun-rpc-portmap-udp;
    application junos-sun-rpc-walld-tcp;
    application junos-sun-rpc-walld-udp;
}
application-set junos-sun-rpc-any {
    application junos-sun-rpc-tcp;
    application junos-sun-rpc-udp;
    application junos-sun-rpc-any-tcp;
    application junos-sun-rpc-any-udp;
}
```

