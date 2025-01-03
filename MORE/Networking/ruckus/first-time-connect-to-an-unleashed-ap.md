# First time connect to an Unleashed AP

- [First time connect to an Unleashed AP](#first-time-connect-to-an-unleashed-ap)
  - [Overview:](#overview)
  - [Connect:](#connect)
  - [Configure First AP](#configure-first-ap)
    - [Define the AP Name:](#define-the-ap-name)
    - [Setup the Mgmt IP (and DHCP settings):](#setup-the-mgmt-ip-and-dhcp-settings)
    - [Define the SSID:](#define-the-ssid)
    - [Define admin account:](#define-admin-account)
    - [Confirm and finish:](#confirm-and-finish)
  - [Reconnect to new AP:](#reconnect-to-new-ap)
    - [Reconnect to the new SSID](#reconnect-to-the-new-ssid)


## Overview: 
This goes over the process for how to connect to a Ruckus Unleashed AP, and what steps to follow to do a first time setup

## Connect: 
When you power on an Unleashed OS on a Ruckus AP, you will be shown a wifi SSID called Configure-Me-XXXXXX.  Select it and open up a browser.  

<img src="img/fst01.png" width="200" alt="">

## Configure First AP
If your browser doesn't automatically connect, in your browser go to the url: `unleashed.ruckuswireless.com`

accept the unsigned cert of the web page, and you will get to the initial spash screen from the AP: 

<img src="img/fst02.png" width="600" alt="">

Select **Create New Unleashed Network**, and then select **Start**.  

<img src="img/fst03.png" width="700" alt="">

Once it's done, confirm you are still connected to the AP (`Configure-Me-XXXXXX`) and select Next. 

<img src="img/fst04.png" width="700" alt="">

### Define the AP Name: 

<img src="img/fst05.png" width="700" alt="">

### Setup the Mgmt IP (and DHCP settings): 

<img src="img/fst06.png" width="700" alt="">

### Define the SSID: 

<img src="img/fst07.png" width="700" alt="">

### Define admin account: 

<img src="img/fst08.png" width="700" alt="">

### Confirm and finish: 

<img src="img/fst09.png" width="700" alt="">

## Reconnect to new AP: 
Once the new settings have been made, you will need to reconnect to your new config.  

When the new settings are set, the Ruckus will show you the following page.  

<img src="img/fst10.png" width="700" alt="">

### Reconnect to the new SSID

<img src="img/fst11.png" width="200" alt="">

And select the mgmt link https://unleashed.ruckuswireless.com/ to manage the AP and bring up the following page. (note that the username/password was defined above in the define admin account section).  

<img src="img/fst02.png" width="600" alt="">

