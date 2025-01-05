# Build a FW Server (R77.30)

## Overview: 
You can get checkpoint firewalls as VM images.  To configure them, follow the following: 

## Getting the VM image installed: 
At the grub page, select to install Gaia on this system

<img src="img/fw01.png" width="500" alt="">

From the Welcome screen, select OK and move forward

<img src="img/fw02.png" width="400" alt="">
<p>

<img src="img/fw03.png" width="400" alt="">

Select the partitions that you want for this firewall

<img src="img/fw04.png" width="400" alt="">

Define the admin password

<img src="img/fw05.png" width="400" alt="">

Then  choose what interface should be the management interface

<img src="img/fw06.png" width="400" alt="">

Configure the IP for the management interface

<img src="img/fw07.png" width="400" alt="">

Then select OK to start with the install

<img src="img/fw08.png" width="400" alt="">

Then wait a long time until the host is configured.  

Once complete, reboot the server

<img src="img/fw09.png" width="400" alt="">

and then at the command prompt, log in to confirm that it's all set: 

<img src="img/fw10.png" width="700" alt="">





## Configure via browser and Wizard
From a web browser, connect to the firewall over https

<img src="img/fw11.png" width="400" alt="">

And select next at the first time wizard

<img src="img/fw12.png" width="400" alt="">

Continue...

<img src="img/fw13.png" width="400" alt="">

Confirm that the network settings are correct for the management interface

<img src="img/fw14.png" width="400" alt="">

Continue through the management connection page (this will get setup later)

<img src="img/fw15.png" width="400" alt="">

Define the hostname, domain, and DNS names

<img src="img/fw16.png" width="400" alt="">

Define NTP settings

<img src="img/fw17.png" width="400" alt="">

only setup a security gateway (since this is just a firewall)

<img src="img/fw18.png" width="400" alt="">

And configure the security gateway, and have it auto download updates.  

<img src="img/fw19.png" width="400" alt="">
<p>

<img src="img/fw20.png" width="400" alt="">

Don't set up the firewall to auto define IPs

<img src="img/fw21.png" width="400" alt="">

and set the one time key to be used between the firewall and the SMS server.  

<img src="img/fw22.png" width="400" alt="">

And then finish the setup 

<img src="img/fw23.png" width="400" alt="">

and wait for the install to get completed.  

One it's complete, log into the firewall 

<img src="img/fw24.png" width="400" alt="">

Then you'll get to the login page for the firewall

<img src="img/fw25.png" width="700" alt="">




## Configure via browser 
From the main web page from before, 

Select network interface, and the eth1 interface, and then select edit

<img src="img/fw26.png" width="700" alt="">

press ok when you get the error, and then give a comment for the internal interface, and select ok. 

<img src="img/fw27.png" width="400" alt="">

Select another interface to edit (select the interface, and press edit) to define another interface

<img src="img/fw28.png" width="400" alt="">

When you're done, you should have all your interfaces complete

<img src="img/fw29.png" width="700" alt="">

Define routes by going to Network Management :: IPv4 Static Routes, select the default route and press edit.  

<img src="img/fw30.png" width="700" alt="">

You can modify the default gateway by either adding a new gateway with the Add Gateway button, or modify the existing gateway with the edit button.  When you do, you will get a popup that provides you a space to define the IP, and it's route priority.  

<img src="img/fw31.png" width="400" alt="">

Modify the banner message by selecting System Management :: Messages  and then editing the banner field.  

<img src="img/fw32.png" width="600" alt="">

Then log out and re-log back in to confirm the message looks fine.  

<img src="img/fw33.png" width="400" alt="">

