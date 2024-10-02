# Mag2600

<img src="../img/mag2600.jpeg">

## Getting Ready:
- [Virginize the box](virginize-the-box.md): how to clean up a dirty box
- [Reset the password](reset-the-password.md): what to do if you goof
- [First time power on](first-time-power-on.md): Giving it a base IP
- [Upgrade OS](upgrade-os.md): Get the box to the newest stable release.  
- [Uploading a Signed Cert](uploading-a-signed-cert.md): Add a SSL cert
- [Configuring the second interface](configuring-the-second-interface.md): Get the 'outside" interface configured too. 

## Base Config: 

### Overviews:
- [Basic setup of a MAG](basic-setup-of-a-mag.md): How all the specific steps go together to configure the MAG. 
- [How to setup VPN Connect](how-to-setup-vpn-connect.md): what steps you need to follow to get vpn connect working.

### User Role:
- [Basic User Role](basic-user-role.md): How to define a basic user role. 
- [Session Timeout](session-timeout.md): How to modify how long till the session gets dropped
- [VPN Tunnels](vpn-tunnels.md): How to enable VPN Tunneling (IPSEC or SSL)

### Resource Policy:
- [Basic Resource Profile](basic-resource-profile.md): How to create a basic resource policy
- [VPN Tunnels - Resource Policies](vpn-tunnels---resource-policies.md): How to continue setting up VPN Tunneling (IPSEC or SSL)

### Authentication Server:
- [Creating an Internal Auth server](creating-an-internal-auth-server.md): Use the MAGs built in DB to store user credentials
- [Linking to a RSA SecurID server](linking-to-a-rsa-securid-server.md): So that you can have 2-factor authentication. 

### Authentication Realm:
- [Building a basic Auth Realm](building-a-basic-auth-realm.md): How to put together the Auth Server, Resource Profile and User Roles.

### Sign-in Policy:
- [Create a basic Sign In Policy](create-a-basic-sign-in-policy.md): create a uri path, and how users will be authenticated on that page.
- [Customize the Login Page](customize-the-login-page.md): fancy up the login page with new colors and images

## References: 
- [Secure Access 7.1](http://www.juniper.net/techpubs/en_US/sa7.1/information-products/pathway-pages/sa-series/index71.html)