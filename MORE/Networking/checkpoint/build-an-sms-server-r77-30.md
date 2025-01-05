# Build an SMS Server (R77-30)

- [Build an SMS Server (R77-30)](#build-an-sms-server-r77-30)
  - [Overview:](#overview)
  - [Installing the Image:](#installing-the-image)
  - [Configure SMS via web GUI and Wizard](#configure-sms-via-web-gui-and-wizard)
  - [Configure SMS via web GUI](#configure-sms-via-web-gui)


## Overview: 
The SMS (Security Management Server) is the database that contains all the firewall rules for all of the checkpoints.  You need to build this before you can configure the firewalls themselves.  

Assumption is that your building one in a VM, but you can also do this on their physical hardware. 




## Installing the Image: 
At the grub window, select "install Gaia on this system"

<img src="img/sms01.png" width="600" alt="">

At the welcome screen, select "ok"

<img src="img/sms02.png" width="500" alt="">
<p>
<img src="img/sms03.png" width="500" alt="">

Modify partitions as needed

<img src="img/sms04.png" width="500" alt="">

Enter a password for user "admin" 

<img src="img/sms05.png" width="500" alt="">

Define the IP address for the primary interfaces

<img src="img/sms06.png" width="500" alt="">

Select OK to continue

<img src="img/sms07.png" width="500" alt="">

Wait a long time for the system to install.....

Reboot the system (removing any CDs as needed)

<img src="img/sms08.png" width="500" alt="">

Login when you get the prompt to confirm the host is installed properly.  

<img src="img/sms09.png" width="700" alt="">





## Configure SMS via web GUI and Wizard
Connect to the SMS server via a web browser (via https)

<img src="img/sms10.png" width="700" alt="">

First time you connect, you'll be brought to the Configuration Wizard.  

<img src="img/sms11.png" width="400" alt="">

Continue with the R77.30 install

<img src="img/sms12.png" width="400" alt="">

Confirm that the network interface is configured properly

<img src="img/sms13.png" width="400" alt="">

Define the hostname/domain for the box, and what it's DNS servers are

<img src="img/sms14.png" width="400" alt="">

Configure NTP 

<img src="img/sms15.png" width="400" alt="">

Configure the unit just to be a Security Manager,  

<img src="img/sms16.png" width="400" alt="">

And within the Products, just select Security Management.  Make sure that the unit is Primary, and that It will automatically download new updates.

<img src="img/sms17.png" width="400" alt="">

 

Next, define the account for the SmartConsole (or the account used when connecting to the database).  

<img src="img/sms18.png" width="400" alt="">

Define what hosts are allowed to connect to the Database of the SMS and make changes: 

<img src="img/sms19.png" width="400" alt="">

You probably don't need to send data to checkpoint (unselect that) and press Finish

<img src="img/sms20.png" width="400" alt="">

Then wait a bit longer for the system to configure....

And then when your done, your get the webUI for the firewall. 

<img src="img/sms21.png" width="700" alt="">




## Configure SMS via web GUI 
Confirm that the default routes are set properly under Network Management :: IPv4 Static Routes   

<img src="img/sms22.png" width="700" alt="">

You can get to the messages page either via the search window, or via System Management :: Messages   

<img src="img/sms23.png" width="700" alt="">

<img src="img/sms24.png" width="700" alt="">

Here you can modify the banner message and the message of the day.

<img src="img/sms25.png" width="700" alt="">

Then log out, and re log in and confirm the message works: 

<img src="img/sms26.png" width="400" alt="">

