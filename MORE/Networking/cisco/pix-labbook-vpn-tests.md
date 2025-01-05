# lab notes testing the vpn between the pixes.

In this lab, we are looking at setting up a vpn between two pix firewalls.  The firewalls are separated by a router that is preventing any icmp, tcp or udp traffic, thus showing that the traffic is being tunneled.  (also there is a sniffer at the middle router showing exactly what the traffic being passed really is.)  At the ends of the network are two routers.  These are the two end points that are communicating through the vpn.

The test works properly and everything works well.  The only thing to note is that the first time a communication is attempted, there is a delay as the vpn tunnel is created.  This could be a problem in the case of a newly created ssh connection.  The first time it is used, it might fail, as the connection is not fully created yet.  This could be prevented by having a continuous ping sent through the vpn to always keep it up.

wiring diagram
```
                                             devlab switch
                                             /-----------\
                            [0003.6bec.a3e0] |           |
                              10.11.13.181   |           | [255.255.255.248]
                            / ---------------+ vlan 13   |  def:10.100.101.166
                            |                \-----------/
                            |                           
                            |                         netlab switch            
                            |                (11.cy3000-1.netlab.cmed.com)
                            |                        /-----------\
(ssh 40.cy3000-devlab-1)    |fe0/0                   |           |
		                    |   /---+----\ fa0/0.33  |           |
		                    \---+ demark +-----------+ vlan 33   |
			                    \--------/    [3/24] | *     *   |
			                          10.100.100.33  | *     *   |
                    			    [              ] | *     *   |  10.100.100.(32-47)
					                                 | *     *   |  [255.255.255.240]
					                                 | *     *   |  
					                                 | *     *   |  
			                        [0003.6bf6.d128] | *     *   | 
			                          10.100.100.34  | *     *   | 
	        (inside)    / ---------------------------+ vlan 33   | 
	(09.cy3000-1.netlab)|e1                   [3/3]  |           |
		                |   /---+----\               |           |
	       	            \---+ pix1   +---------------+ vlan 10   |
		                    \--------/e0      [3/4]  | *     *   |
		                (outside)     10.100.100.5   | *     *   |
			                        [0003.6bf6.d127] | *     *   |  10.100.100.(4-7)
                            					     | *     *   |  [255.255.255.252]
                    			    [0003.6bec.a3c0] | *     *   |
                    			      10.100.100.6   | *     *   |
	       		            /------------------------+ vlan 10   |
	      4.cy3000-1.netlab)|fa0/0.10 [2/3]          |           |
		                    |   /---+----\ fa0/0.11  |           |
		                    \---+router1 +-----------+ vlan 11   | 
			                    \--------/    [2/3]  | *     *   |
			                          10.100.100.9   | *     *   |
			                        [0003.6bec.a3c0] | *     *   |  10.100.100.(8-11)
					                                 | *     *   |  [255.255.255.252]
					                                 | *     *   |  
					                                 | *     *   |  
			                        [0050.54ff.e38a] | *     *   |
			                          10.100.100.10  | *     *   |
		        (outside)   /------------------------+ vlan 11   |
	    (10.cy3000-1.netlab)|e0               [3/1]  |           |
		                    |   /---+----\           |           |
	                	    \---+ pix2   +-----------+ vlan 12   | 
			                    \--------/e1  [3/2]  | *     *   |
		                (inside)      10.100.100.13  | *     *   |
			                        [0050.54ff.e38b] | *     *   |  10.100.100.(12-15)
	            (15.cy3000-1.netlab)                 | *     *   |  [255.255.255.252]
		                   |   /--------\ fa0/0.12   | *     *   |
		                   \---+router2 +------------+ vlan 12   |
	                	       \--------/     [2/2]  |           |
		                    	      10.100.100.14  |           |
	                    		    [0003.e3c4.db00] |           |
		                            			     \-----------/
```

With the initial setup, you should be able to ping from the demark to router2 and back. It should be a known good setup to then attach the vpn setup on top of.

# switch: 
set vlan 11 3/1 
set vlan 12 3/2 
set vlan 33 3/4 
set vlan 10 3/3 
set port speed 3/1 100 
set port duplex 3/1 full 
set port speed 3/2 100 
set port duplex 3/2 full 
set port speed 3/3 100 
set port duplex 3/3 full 
set port speed 3/4 100 
set port duplex 3/4 full 

## (router ports already setup as trunks, and vlans already created)
```
# pix 1:
	conf t
	 hostname pix1
	 interface ethernet0 100full
	 interface ethernet1 100full
	 ip address outside  10.100.100.5  255.255.255.252
	 ip address inside 10.100.100.34 255.255.255.240
	 access-list acl-inbound permit icmp any any
	 access-group acl-inbound in interface outside
	 nat (inside) 0 10.0.0.0  255.0.0.0 0 4096
	 route inside 0.0.0.0 0.0.0.0 10.100.100.33
	 route outside 10.100.100.8  255.255.255.252 10.100.100.6
	 route outside 10.100.100.12 255.255.255.252 10.100.100.6
	 logging on
	 logging timestamp
	 logging buffered errors
	 exit
	wr mem
# demark router:
	conf t
	 ip route 10.100.100.0 255.255.255.224 10.100.100.34
	 ip route 10.100.100.4  255.255.255.252 10.100.100.34
	 ip route 10.100.100.8  255.255.255.252 10.100.100.34
	 ip route 10.100.100.12 255.255.255.252 10.100.100.34
	 exit
# pix 2:
	conf t
	 hostname pix2
	 interface ethernet0 100full
	 interface ethernet1 100full
	 ip address outside 10.100.100.10 255.255.255.252
	 ip address inside  10.100.100.13 255.255.255.252
	 access-list acl-inbound permit icmp any any
	 access-group acl-inbound in interface outside
	 nat (inside) 0 10.100.100.12 255.255.255.252 0 4096
	 route outside 0.0.0.0 0.0.0.0 10.100.100.9
	 logging on
	 logging timestamp
	 logging buffered errors
	 exit
	wr mem
# router1:
	conf t
	 hostname router1
	 interface FastEthernet0/0
	  speed 100
	  duplex full
	  no shut
	  exit
	 interface FastEthernet0/0.10
	  des outside int, toward pix1
	  encap dot1q 10
	  ip address 10.100.100.6 255.255.255.252
	  no shut
	  exit
	 interface FastEthernet0/0.11
	  des inside int, toward pix2
	  encap dot1q 11
	  ip address 10.100.100.9 255.255.255.252
	  no shut
	  exit
	 ip route 0.0.0.0 0.0.0.0 10.100.100.5
	 ip route 10.100.100.12 255.255.255.252 10.100.100.10
	 exit
	wr mem
# router2:
	conf t
	 hostname router2
	 interface FastEthernet0/0
	  no shut
	  exit
	 interface FastEthernet0/0.12
	  des outside int, toward pix2
	  encap dot1q 12
	  ip address 10.100.100.14 255.255.255.252
	  no shut
	  exit
	 ip route 0.0.0.0 0.0.0.0 10.100.100.13
	 exit
	wr mem
```
```
# pix1:
	conf f
	 !
	 passwd password
	 enable password wickedsecurepass
	 domain-name netlab.cmed.com
	 ca generate rsa key 1024
	 !
	 ca save all
	 wr mem
	 !
	 isakmp key givemethekeyscocksuckerwhatthefuck address 10.100.100.10
	 isakmp policy 10 encrypt aes-256
	 isakmp policy 10 hash sha
	 isakmp policy 10 authentication pre-share
	 isakmp policy 10 group 5
	 isakmp policy 10 lifetime 86400
	 isakmp enable outside
	 !
	 access-list 110 permit ip 10.100.100.32 255.255.255.240 10.100.100.12 255.255.255.252 
	 crypto ipsec transform-set temptransform esp-aes-256
	 crypto map tempmap 10 ipsec-isakmp
	 crypto map tempmap 10 match address 110
	 crypto map tempmap 10 set peer 10.100.100.10
	 crypto map tempmap 10 set transform-set temptransform
	 crypto map tempmap 10 set pfs group5
	 crypto map tempmap interface outside
	 sysopt connection permit-ipsec
	 !
	 exit
	wr mem
 
# router 1 ("in between" router):
	conf t
	 no ip access-list extended ipfilter
	 ip access-list extended ipfilter
	  remark ### traffic out pix2 -> pix1 ###
	  permit tcp host 10.100.100.10 host 10.100.100.5
	  permit udp host 10.100.100.10 host 10.100.100.5
	  permit icmp host 10.100.100.10 host 10.100.100.5
	  permit 50 host 10.100.100.10 host 10.100.100.5
	  permit 51 host 10.100.100.10 host 10.100.100.5
	  remark ### traffic in pix1 -> pix2 ###
	  permit tcp host 10.100.100.5 host 10.100.100.10
	  permit udp host 10.100.100.5 host 10.100.100.10
	  permit icmp host 10.100.100.5 host 10.100.100.10
	  permit 50 host 10.100.100.5 host 10.100.100.10
	  permit 51 host 10.100.100.5 host 10.100.100.10
	  remark ### allow anything else ###
	  permit ip any any log
	  exit
	 interface FastEthernet0/0.10
	  ip access-group ipfilter in
	  exit
	 interface FastEthernet0/0.11
	  ip access-group ipfilter in
	  exit
	 exit 
	wr mem
 
# pix2:
	conf f
	 !
	 passwd password
	 enable password wickedsecurepass
	 domain-name netlab.cmed.com
	 ca generate rsa key 1024
	 !
	 ca save all
	 wr mem
	 !
	 isakmp key givemethekeyscocksuckerwhatthefuck address 10.100.100.5
	 isakmp policy 10 encrypt aes-256
	 isakmp policy 10 hash sha
	 isakmp policy 10 authentication pre-share
	 isakmp policy 10 group 5
	 isakmp policy 10 lifetime 86400
	 isakmp enable outside
	 !
	 access-list 110 permit ip 10.100.100.12 255.255.255.252 10.100.100.32 255.255.255.240
	 crypto ipsec transform-set temptransform esp-aes-256
	 crypto map tempmap 10 ipsec-isakmp
	 crypto map tempmap 10 match address 110
	 crypto map tempmap 10 set peer 10.100.100.5
	 crypto map tempmap 10 set transform-set temptransform
	 crypto map tempmap 10 set pfs group5
	 crypto map tempmap interface outside
	 sysopt connection permit-ipsec
	 !
	 exit
	wr mem
```

- Configuring IPSec and Certification Authorities http://cisco.com/univercd/cc/td/doc/product/iaabu/pix/pix_sw/v_63/config/ipsecint.htm 
- Site-to-Site VPN Configuration Examples http://cisco.com/univercd/cc/td/doc/product/iaabu/pix/pix_sw/v_63/config/sit2site.htm