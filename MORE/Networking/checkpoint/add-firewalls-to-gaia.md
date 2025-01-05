# Add firewalls to GAIA
open up the smart dashboard

<img src="img/gaia01.png" width="600" alt="">





## Create a new firewall object
start in the gaia portal and select 

Network Objects :: Check Point (right click):: Check Point :: Security Gateway/Management...

<img src="img/gaia02.png" width="400" alt="">

first time you do this, you need to confirm that you want classic mode, and to not see the annoyance message again

<img src="img/gaia03.png" width="200" alt="">

In the classic mode of the gateway, make sure you define the following: 

- name of the firewall
- primary IP of firewall
- comment to describe the firewall
- color of the firewall's icon in the smart dashboard
- what blades the firewalls should run

<img src="img/gaia04.png" width="600" alt="">

Then setup communications between the database (gaia) and the firewall by setting up SIC communications by pressing the communications (6) button 

This will pull up the SIC window where you enter in the SIC one time password (1) that you defined when you first setup the firewall, and then setup the connection by pressing the Initialize (2) button.  

<img src="img/gaia05.png" width="400" alt="">

when the connection is set, you will see the state enabled (3) and then you can press ok (4) to setup.  

<img src="img/gaia06.png" width="400" alt="">

It will then show you the interfaces (topology) of that firewall which you can close out to continue

<img src="img/gaia07.png" width="400" alt="">

Then you can confirm that the state with the new gateway is trusted (1)

<img src="img/gaia08.png" width="600" alt="">

Modify the interface settings by selecting Topology (1) and then selecting one of the interfaces (double clicking or single click & Edit)

<img src="img/gaia09.png" width="600" alt="">

In this case, we will want to define the interface as a DMZ network, so select Topology (1) and select the Interface leads to DMZ (2).  

<img src="img/gaia10.png" width="400" alt="">

by ok'ing the changes, you will get back to the overview page, that now includes the new firewall in the "my organization" section (1) and also a new firewall in the Checkpoint (2) section. 

<img src="img/gaia11.png" width="600" alt="">





## Create a new host node
Under Network Objects :: Nodes (right click) :: Node :: Host...

<img src="img/gaia12.png" width="400" alt="">

For that host device, define 

- name for the host
- IP address
- additional comment describing the object
- color to use for the icon in the gui

<img src="img/gaia13.png" width="600" alt="">

once you commit that object (by pressing ok) you will see the object show up in the Nodes (1) section in the lower left corner of the SmartDashboard.

<img src="img/gaia14.png" width="200" alt="">





## Create a network object
you can also define a network object like a vlan.  to do this, select Network Objects :: Network (right click) :: Network...

<img src="img/gaia15.png" width="400" alt="">

Define the network object by modifying the following fields

- name of the network
- comment or description of the network
- color of the network icon when show in gaia
- network IP
- network mask

<img src="img/gaia16.png" width="600" alt="">

once the object is created, you will be able to see it in the bottom left of the SmartDashboard under Networks: 

<img src="img/gaia17.png" width="200" alt="">




## Creating Rules (ACLs) 
In the SmartDashboard, select Policy (1), then add a new policy (2), and a new line will appear (3) in the policy window.  

<img src="img/gaia18.png" width="600" alt="">


### Cleanup Rule: 
The checkpoint has default (implied) acl rules that are not shown.  One is that there is a default deny that does not log, which can be tricky when you're troubleshooting because packets are being dropped and you don't know why.  To fix this, let's define a default deny acl statement that logs.

Modify the new policy by double clicking (1) on the blank fields to modify the name field 

<img src="img/gaia19.png" width="600" alt="">

right click on the Track field and select Log to log every time this policy gets a hit.  

<img src="img/gaia20.png" width="600" alt="">




### Management Rule: 
You also want a rule that will allow access to the firewalls from the SMS host and the admin's hosts.  

Add a new policy with the add rule on top (1) button, then enter in the name (2) like noted before

<img src="img/gaia21.png" width="600" alt="">

There are a couple of different ways to enter objects into the source and destination fields.  One way, is to select and drag the icons directly onto the field.  

<img src="img/gaia22.png" width="600" alt="">

Another way is to click the little plus symbol in the field to add (1), then enter in part of the object's description (2) in the search field, and then select the object you are looking for (3) 

<img src="img/gaia23.png" width="600" alt="">

You can use the (+) method for adding objects in the service and other fields as well.  

<img src="img/gaia24.png" width="150" alt="">

Right click on the Action field to change the Drop to Accept

<img src="img/gaia25.png" width="600" alt="">

And for the management rule, also make sure that you track hits to the rule by sending to log

<img src="img/gaia26.png" width="600" alt="">




## Modify Implied (default) Firewall ACL rules 

<img src="img/gaia27.png" width="50" alt="">

Lets modify the default firewall rules to allow the them to reply to ICMP.  To do this, select the Launch Menu Icon () and select Policy :: Global Properties... 

<img src="img/gaia28.png" width="600" alt="">

Under Firewall (1) check Accept ICMP request (2) and select the First (3) pulldown.  Select Log Implied Rules (4) and ok(5).  

<img src="img/gaia29.png" width="600" alt="">




## Save Policy: 

<img src="img/gaia30.png" width="50" alt="">

The rules you have entered are not saved by default, so you need to save them by clicking the save icon (), or File :: Save As

<img src="img/gaia31.png" width="400" alt="">

Then name your saved configs, and press ok.  

<img src="img/gaia32.png" width="400" alt="">

You will then see the name of the saved change in the window

<img src="img/gaia33.png" width="600" alt="">




## Installing The Policy: 
To push these changes to the firewall, you will need to select the "install policy" button in the SmartDashboard window.  

<img src="img/gaia34.png" width="600" alt="">

Make sure that you are selecting the correct gateway (1) , and make sure that you are saving a version of the database (2) and then select ok to compile the change. 

<img src="img/gaia35.png" width="600" alt="">

once the revision has been compiled, it will then be pushed to the firewalls.  

<img src="img/gaia36.png" width="600" alt="">



## Adding a Remote Firewall (gateway) 
Here, we are trying to add a remote firewall.  The trick here is that you need to define the remote firewall and update the local firewall, then you can link up the SIC connection.  

<img src="img/gaia37.png" width="300" alt="">

Create a new firewall like defined above.  

When you select the SIC communication button (1), enter in the SIC one time password (2) like before, and then press Initialize (3) to attempt the connection.  

<img src="img/gaia38.png" width="600" alt="">

That connection will fail because the local firewall does not know about the remote one.  

<img src="img/gaia39.png" width="300" alt="">

Then select CANCEL (not ok)

<img src="img/gaia40.png" width="600" alt="">

And click yes to resetting the SIC.  

Click OK at the new firewall/gateway's properties page, and accept the warning 

<img src="img/gaia41.png" width="300" alt="">

Then push the policy

<img src="img/gaia42.png" width="300" alt="">

But here, only push the change to the "A" (local) firewall, and deselect the "B" gateway for the update (because you can't connect to it yet).  But this will push the update to the "A" gateway, allowing it to know about the "B".  

<img src="img/gaia43.png" width="600" alt="">

Once the policy is pushed to the local firewall, then re-edit the remote "B" object, test the communications (1), enter in the SIC passcode (2), test (3), and confirm. 

<img src="img/gaia44.png" width="600" alt="">

