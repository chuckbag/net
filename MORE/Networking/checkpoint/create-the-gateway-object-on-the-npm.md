# Create the Gateway object on the NPM

## Set the management interface
Since your going to manage the firewall remotely, you want to change the management interface to the external int.  

From the interface window, select Set Management Interface: 

<img src="img/npm01.png" width="700" alt="">

Change the interface to eth1

<img src="img/npm02.png" width="700" alt="">

And click ok. 

<img src="img/npm03.png" width="700" alt="">



## Create new object in NPM: 
Under Network Objects, Chdkpoint (1) select Checkpoint (2) and Security Gateway/Management (3)

<img src="img/npm04.png" width="400" alt="">

For the new firewall definition, add the name, type of platform and it's IP.  

<img src="img/npm05.png" width="500" alt="">

Enter the SIC password that you created when you first ran the setup wizard on the firewall itself. 

<img src="img/npm06.png" width="500" alt="">

If all goes well, the NPM will connect to the firewall, and start communicating using the SIC password for authentication.  

<img src="img/npm07.png" width="500" alt="">




## Setup new policy: 
Install the blades you want for your new firewall: 

<img src="img/npm08.png" width="500" alt="">

Save the configs: 

<img src="img/npm09.png" width="200" alt="">

Create a new encryption domain by creating the group and the network range 

File :: Manage :: Network Objects :: New :: Group :: Simple Group

<img src="img/npm10.png" width="500" alt="">

Create the network Object


<img src="img/npm11.png" width="400" alt="">

   
<img src="img/npm12.png" width="400" alt="">

Set OK to create the group

<img src="img/npm13.png" width="500" alt="">

Save the configs to the new policy package


<img src="img/npm14.png" width="400" alt="">

<p>

<img src="img/npm15.png" width="300" alt="">

Set the installation targets for the new policy.  

<img src="img/npm16.png" width="400" alt="">

Make sure that this new policy only includes the new lab firewall: 

<img src="img/npm17.png" width="400" alt="">




## Finish setting up the Firewalls policy
Create a VPN Community: 

Under IPSec VPN, select new

<img src="img/npm18.png" width="700" alt="">

Then in the popup window, provide the following information: 
```
General: 
  Name = LXI_LAB_VPN
  Comment = From lexington to lab
  color = red
Center Gateway
  LEX-CLUSTER-01
Satellite Gateway
  lxi-labfw3
Encryption
  IKEv1 for IPv4
  Custom :: p1: AES-256, SHA-256. p2: AES-128, SHA-256
Tunnel Management: 
  One Tunnel per subnet
Advanced Settings :: VPN Routing
  To center only (selected)
Advanced Settings :: Advanced VPN Properties
  Diffie-Hellman :: group2
  IKE sec associations 1440min
  IPsec sec associations 3600sec
  disable nat inside of vpn (selected)
```

Then add the route domain into the firewall

<img src="img/npm19.png" width="700" alt="">

Then go through the firewall rules and make sure that it is what you want.

<img src="img/npm20.png" width="700" alt="">

Then Save and Push Policy: 

<img src="img/npm21.png" width="400" alt="">

