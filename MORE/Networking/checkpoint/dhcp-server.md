# DHCP Server

Login in directly to the firewall, and in the view mode (1) select advanced.  Then under Network Management, select DHCP Server(2).  Make sure you select Enable DHCP Server (3) and enable it (4).  Then select Add (5) to create a new scope.  

<img src="img/dhcp01.png" width="700" alt="">

Make sure you select the Enable DHCP Checkbox, then enter the Network address and subnet mask for the network you want served.  This is not the firewall interface, but the actual network of that interface.   Under Address Pool, select Add to create the range to dish out.  

<img src="img/dhcp02.png" width="500" alt="">

Enable the range, and select the IP range that IPs should be given

<img src="img/dhcp03.png" width="300" alt="">

Then in the Routing & DNS tab, define what the default gateway will be, the domain, and what the DNS Servers should be.  

<img src="img/dhcp04.png" width="500" alt="">

