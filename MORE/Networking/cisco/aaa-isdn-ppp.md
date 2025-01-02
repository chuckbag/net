# AAA + ISDN + PPP:

- [AAA + ISDN + PPP:](#aaa--isdn--ppp)
  - [Overview:](#overview)
  - [Setup Basic, Global AAA](#setup-basic-global-aaa)
    - [Enabling AAA](#enabling-aaa)
  - [Enabling AAA on an Interface](#enabling-aaa-on-an-interface)
    - [setting up an interface with aaa](#setting-up-an-interface-with-aaa)
  - [Adding ISDN Dial In Access](#adding-isdn-dial-in-access)
    - [ISDN Dial In](#isdn-dial-in)
  - [Enabling Radius Authenticated ISDN Connections](#enabling-radius-authenticated-isdn-connections)
    - [ISDN Dial In with Radius](#isdn-dial-in-with-radius)
  - [Enabling ISDN PPP Connections](#enabling-isdn-ppp-connections)
    - [ISDN with PPP](#isdn-with-ppp)
  - [Enabling PPP AAA Authentication: Globally and per Interface](#enabling-ppp-aaa-authentication-globally-and-per-interface)
    - [ISDN with PPP and AAA](#isdn-with-ppp-and-aaa)
  - [Appendix A - Only allowing ppp, not console](#appendix-a---only-allowing-ppp-not-console)
    - [PPP without console access](#ppp-without-console-access)


## Overview: 
In this document we attempt to setup a router with an ISDN modem, so that it can receive console and ppp connections. We will also see how to have these connections authorized by the local radius server.

While doing this we will point out the basic configurations for AAA and how it works with a radius server. We will look at how to configure an ISDN and modem line cards to allow dial in access to a router. And we will look at how to enable ppp on dialin connections.

Note that all the commands are hyperlinked to their specific definitions on cisco's webpage. Some of these links are to locations that can only be accessed via a cisco accounts. (sorry)

## Setup Basic, Global AAA
Here we are setting up the most basic aaa config. We are enabling AAA, and we are defining how logins should be handled. We have created two groups (lines 8-9) but we also setup a default auth for all logins that have not been assigned to one of those groups. (Which at this point is all means of logging in.)

### Enabling AAA

```
01  ! -- standard router local password
02  username chuck password letmein     
03  enable secret 5 asdlfkjsad;kljf 
04  !
05  ! -- global aaa config
06  aaa new-model
07  aaa authentication login default line 
08  aaa authentication login local_group local	 
09  aaa authentication login radius_group group radius 
10  !
11  ! -- line command (vty)
12  line vty 0 4  
13   password cisco 
14  !
15  ! -- specify the radius server
16  radius-server host 172.22.53.201
```

A quick highlight on some of the commands listed above include:
- Line 6, turns on aaa
- Line 7, allows interfaces not associated with aaa groups to use their default line logins. Thus with this example, only the vty line is enabled (you would not be able to connect via the console port) and since the vty line is not associated to a group, then it would use it's line password Line 13.
- Line 8, creates a group called "local_group". All future connections that refer to the group "local_group" will be authorized by the local password (Line 2).
- Line 9, creates a group called "radius_group". All future connections that refer to it, will be authenticated to one of the defined radius servers. We only have one radius server defined, and that's on line 16.

## Enabling AAA on an Interface
For this example, we will setup the router so that the console port uses the radius server for login authentication. The commands from the previous config's in "Enabling AAA" are assumed to already be entered in the router.

### setting up an interface with aaa

```
17  ! -- console port login
18  line con 0 
19   password dudeletmein 
20   login authentication radius_group
21   transport input none
```

Here we have entered the normal config's to enable the console port, but we have also added the "`login authentication {group}`" command (line 20).

Normally with the config's above (minus line 20) when you connected to the console, the router would prompt you for a password, and you would enter "`dudeletmein`". But since we added the aaa group to this interface, the default line password (line 19) is now ignored, and instead the radius server is used.

With these two combined config's we have two ways to access the router. The first is to telnet to the router (vty 0 4). The vty server does not have a specified aaa group associated with it, so as per line 7 it will use its default line password line 13. The other way to connect to the router is through the console port. Here we have defined an aaa group, so all connections will not use its default line password (line 19), but instead use the radius server.

For debugging aaa authentication, use the command debug aaa authentication. If you think that you might be having some problems connecting to your radius server, see here for some different debug outputs.

## Adding ISDN Dial In Access
The easiest way to enable dialin to a router is to hang a modem off the AUX port on the router. In the configuration explained below, I have set things up a little different. Instead of POTS lines going into a modem, I have a series of ISDN lines that connect to a blade in my router. This configuration is handy because the isdn allows caller id, and each isdn line is two POTS lines. So with 5 ISDN lines, I can support 10 concurrent calls. To pull this off, I have an [ISDN BRI Network Module](http://www.cisco.com/en/US/customer/products/hw/modules/ps2797/products_module_installation_guide_chapter09186a008007c8f8.html) in the router that the ISDN lines tie into. And rather then having external modems, that wouldn't work with isdn anyway, I have an internal [Digital Modem Network Module](http://www.cisco.com/en/US/customer/products/hw/modules/ps2797/products_module_installation_guide_chapter09186a008007c8e8.html) in the router as well.

To get an idea on how this all works together, see the diagram below.

<img src="img/isdn01.gif" width="500" alt=""> 

The ISDN lines all tie into the ISDN card. It's data is then sent to the modems to be decoded. (The modem card has many modems in it. To clean up the config's in the router, you can group all the modems together into one command; "int Group-Async1".) After the modems decode the calls, the line interface then handles getting the modem call onto the network (or giving it a router cli).

So below we are going to configure; the ISDN blade under the "interface BRI3/0" heading (line 23), the modem blade under the "interface Group-Async1" heading (line 32), and the routers ability to take the modem's output and give it a cli via the "line 33 50" heading (line 38).

### ISDN Dial In

```
22  ! -- Configure the BRI
23  interface BRI3/0 
24   no ip address
25   isdn switch-type basic-ni 
26   isdn spid1 30355500001111 5303227 
27   isdn spid2 30355500011111 5303236 
28   isdn caller 6505551212 
29   isdn incoming-voice modem 
30  ! 
31  ! -- Configure Group-Async
32  interface Group-Async1 
33   ip unnumbered FastEthernet0/0 
34   no peer default ip address
35   group-range 33 50 
36  ! 
37  ! -- Configure Line Ports
38  line 33 50 
39   password wannaletmein 
40   login authentication local_group 
41   modem Dialin
42   transport output none
```

A quick highlight on some of the commands listed above include:
- 25 Set the isdn type. There are a couple of different isdn switch types, but here in the us, it's mostly only the "basic-ni".
- 26-27 Define the isdn phone number. Each isdn line has two spids, the spid is normally the phone number plus 4 digits. So for spid1, it's phone number would be 303-555-0000. All this information is given to you by the telco, when you get your ISDN line.
- 28 Caller ID allow/blocking. With this command, only the number listed, 650-555-1212, is allowed to make connections. All other call sources will get a busy signal. You can have as many of these commands in a row as necessary.
- 34 Don't give modem connection and IP. Here we are only dialing in to get cli access. So we don't need to get our own ip address (as in a ppp connection)
- 35 Group the modems. This command takes the 17 modems and combines them into one entity in the config. This makes it much easier to configure, because instead of making config statements to each modem, we can make one statement to all the modems.
- 40 Authenticate logins on interface to aaa group. This sets the interface to use the specified aaa group for deciding how to authenticate connections.
- 41 Allow modem connections. This allows the modems to connect the line interfaces. This is needed so that something is "connected" to the other side of the modem. (isdn on one side, line interfaces on the other).

## Enabling Radius Authenticated ISDN Connections
It's very straight forward. (same as any other interface). Just add the "login authentication" command in the line command.

Repeating the above config with the addition of the "login authentication" command:

### ISDN Dial In with Radius

```
22  ! -- Configure the BRI
23  interface BRI3/0 
24   no ip address
25   isdn switch-type basic-ni 
26   isdn spid1 30355500001111 5303227 
27   isdn spid2 30355500011111 5303236 
28   isdn caller 6505551212 
29   isdn incoming-voice modem 
30  ! 
31  ! -- Configure Group-Async
32  interface Group-Async1 
33   ip unnumbered FastEthernet0/0 
34   no peer default ip address
35   group-range 33 50 
36  ! 
37  ! -- Configure Line Ports
38  line 33 50 
39   password wannaletmein 
40   login authentication local_group 
41   modem Dialin
42   login authentication radius_group
43   transport output none
```

Same logic as in the above section Enabling AAA on an interface.

## Enabling ISDN PPP Connections
In this section we are going to enable PPP on the ISDN interfaces, but to keep things simple, we are going to only look at ppp, and not aaa. So the config added in the config "3.2 - ISDN Dial In with Radius" will be removed.

So from the previous config "3.1 - ISDN Dial In", we are adding (in bold) all the configs to make ppp work on the isdn interface.

### ISDN with PPP

```
22  ! -- Configure the BRI
23  interface BRI3/0 
24   ip unnumbered FastEthernet0/0
25   encapsulation ppp
26   isdn switch-type basic-ni 
27   isdn spid1 30355500001111 5303227 
28   isdn spid2 30355500011111 5303236 
29   isdn caller 6505551212 
30   isdn incoming-voice modem 
31   peer default ip address pool dialin_ip_pool
32  ! 
33  ! -- Configure Group-Async
34  interface Group-Async1 
35   ip unnumbered FastEthernet0/0 
36   encapsulation ppp
37   async mode interactive
38   peer default ip address pool dialin_ip_Pool
39   group-range 33 50 
40  ! 
41  ! -- Configure Line Ports
42  line 33 50 
43   password wannaletmein 
44   autoselect during-login
45   autoselect ppp
46   modem Dialin
47   transport input all
48   transport output none 
49  ! 
50  ! -- Create an IP Pool
51  ip local pool dialin_ip_pool 10.1.1.100 10.1.1.110
```

Some comments on the configs above:
- 24 Enable IP processing with no interface ip Allow IP processing, but do not assign an ip address to the interface itself. Instead ppp connections will be assigned their own ip's and traffic will route through the interface.
- 25 Enable PPP on interface.
- 31 Create list of IP's for PPP users. Creates a list of IP's that will be given to ppp connections.
- 37 Allow SLIP and PPP on interface. Allows EXEC prompt to handle PPP and SLIP commands.
- 38 Give PPP and SLIP connections IPs from a group. All SLIP and PPP connections that originate from this line will get IPs from the range listed from the specified pool.
- 44 Send prompt as soon as connection is made. This helps SLIP and PPP, by giving a prompt as soon as the connection is made, rather then waiting for a carriage return to be sent.
- 45 Start Line with PPP. Prevents the user from having to specify to start PPP. The router just starts it for them.
- 47 Allow all protocols. Allow all protocol to be passed through ppp. (allow telnet, ssh, ftp, www, etc)
- 51 Create IP pool. Creates the ip pool to be used for the PPP sessions. The pool range is within 10.1.1.(100-110).

## Enabling PPP AAA Authentication: Globally and per Interface
Putting it all together, this is what you config would be like to get AAA working on both console dialin and PPP access.

### ISDN with PPP and AAA

```
01  ! -- standard router local password
02  username chuck password letmein     
03  enable secret 5 asdlfkjsad;kljf 
04  !
05  ! -- global aaa config
06  aaa new-model
07  aaa authentication login default line 
08  aaa authentication login local_group local 
09  aaa authentication login radius_group group radius 
10  !
11  ! -- console port login
12  line con 0 
13   password dudeletmein 
14   login authentication radius_group 
15   transport input none 
16  !
17  ! -- line command (vty)
18  line vty 0 4  
19   password cisco 
20  ! 
21  ! -- specify the radius server 
22  radius-server host 172.22.53.201 
23  ! 
24  ! -- Configure the BRI
25  interface BRI3/0 
26   ip unnumbered FastEthernet0/0 
27   encapsulation ppp
28   isdn switch-type basic-ni 
29   isdn spid1 30355500001111 5303227 
30   isdn spid2 30355500011111 5303236 
31   isdn caller 6505551212 
32   isdn incoming-voice modem 
33   peer default ip address pool dialin_ip_pool 
34  ! 
35  ! -- Configure Group-Async
36  interface Group-Async1 
37   ip unnumbered FastEthernet0/0 
38   encapsulation ppp
39   async mode interactive
40   peer default ip address pool dialin_ip_Pool 
41   group-range 33 50 
42  ! 
43  ! -- Configure Line Ports
44  line 33 50 
45   password wannaletmein 
46   autoselect during-login 
47   autoselect ppp 
48   login authentication radius_group 
49   modem Dialin
50   transport input all 
51   transport output none 
52  ! 
53  ! -- Create an IP Pool
54  ip local pool dialin_ip_pool 10.1.1.100 10.1.1.110
```

## Appendix A - Only allowing ppp, not console
If you wanted to prevent console access, and only allow ppp access these configs would do that. Also, the chap and pap enabled connections would encrypt the passwords as they traveled between then computer and the access server. We don't worry about this in the above docs because we are dealing with a 2-factor password scheme, and thus the password is constantly changing and is useless after it has been used.

### PPP without console access

```
101  ! -- Config AAA 
102  aaa authentication ppp radius_ppp_group group radius 
103  ! 
104  ! -- Configure BRI
105  interface BRI3/0 
106   ppp authentication chap ms-chap radius_ppp_group 
107  ! 
108  ! -- Configure Group-Async
109  interface Group-Async1 
110   ppp authentication chap ms-chap radius_ppp_group
```

Note that these configs would be added to the configs from "4 - ISDN with PPP and AAA".   