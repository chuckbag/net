# How to setup VPN Connect

Once you have the MAG setup and working, you can grant users access to the Network Connect services.  Network Connect is Junipers name for their SSLVPN and IPSec VPN service.

The following is an outline for the steps you need to follow to allow users to establish vpn tunnels.  This is simply an overview.  The links will show you want steps are needed to accomplish each of these. 

## User Role:
[Modify a USER ROLE to allow vpn tunnels](vpn-tunnels.md).  Generally speaking within a User Role, you can modify the following:
- allow different services for that role including web, file mounts, ssh proxies, terminal services, virtual desktops, meetings, and what we want: VPN Tunneling. 
- you can also define session timeouts
- you can create specific UI's for users that login with this Role
- and within the VPN Tunneling options, you can modify the following:
  - is it ok to use split tunneling?
  - who's routes do you want to prefer (users or vpns)
  - do you want to run any start/stop scripts

## Resource Profile:
You can [create a VPN Tunneling Resource profile to manage all the different features specific to enabling vpn tunnels](vpn-tunnels---resource-policies.md).  They include the following different sub groups.

### Access Control
- This is bound to a User Role
- it defines what IPs a client can connect to (what routes).  These rules can be complex like if you are coming from IP=X then block access to Range=Y. 

### Connection Profile
- This is bound to a User Role
- it lets you set up the DHCP pool IP ranges that clients get
- what kind of tunnels you create (IPSEC or SSL)
- and what DNS and proxy servers should be used. 

### Split Tunneling Networks
- This is bound to a User Role
- lets you define what split tunnel routes will be sent down to the client (if your not doing split tunnel, then the client simply gets a 0.0.0.0 route)

### Bandwidth Management
- This is bound to a User Role
- allows you do bandwidth throttling, and provide minimum bandwidth to clients. 
