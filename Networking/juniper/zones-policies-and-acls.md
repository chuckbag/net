# Zones, Policies, and ACL's

- [Zones, Policies, and ACL's](#zones-policies-and-acls)
  - [Security Zones:](#security-zones)
  - [Address Book:](#address-book)
  - [Application Sets:](#application-sets)
  - [Policy:](#policy)
  - [References:](#references)

## Security Zones: 
```
set security zones security-zone newzone host-inbound-traffic system-services ping
set security zones security-zone newzone interfaces ge-0/0/5.0
set security zones security-zone newzone interfaces ge-0/0/6.0
```

or 
```c
security {
    zones {
        security-zone newzone {
            host-inbound-traffic {
                system-services {
                    ping;
                }
            }
            interfaces {
                ge-0/0/5.0;
                ge-0/0/6.0;
            }
        }
    }
}
```

where
- `host-inbound-traffic`: says what kind of traffic is allowed TO (not though) this device

## Address Book:
Address books are aliases, that allow you to make one single reference to multiple items.  In this example the name "friendlys" refers to both a /24 and a single IP address.

```
set security zones security-zone untrust address-book address office1 5.5.5.5/32
set security zones security-zone untrust address-book address dc1 8.1.1.0/24
set security zones security-zone untrust address-book address-set friendlys address office1
set security zones security-zone untrust address-book address-set friendlys address dc1
```

or
```
security {
    zones {
        security-zone untrust {
            address-book {
                address office1 5.5.5.5/32;
                address dc1 8.1.1.0/24;
                address-set friendlys {
                    address office1;
                    address dc1;
                }
            }
        }
    }
}
```

where 
- "`friendlys`" is an alias to `5.5.5.5/32` and `8.1.1.0/24`   

We will do this again to refer to the inside devices that make up the alias "webTier" and "appTier". 

```
set security zones security-zone newzone address-book address web1 10.0.0.1/32
set security zones security-zone newzone address-book address web2 10.0.0.2/32
set security zones security-zone newzone address-book address app1 10.0.1.1/32
set security zones security-zone newzone address-book address app2 10.0.1.2/32
set security zones security-zone newzone address-book address-set webTier address web1
set security zones security-zone newzone address-book address-set webTier address web2
set security zones security-zone newzone address-book address-set appTier address app1
set security zones security-zone newzone address-book address-set appTier address app2
```

or
```
security {
    zones {
        security-zone newzone {
            address-book {
                address web1 10.0.0.1/32;
                address web2 10.0.0.2/32;
                address app1 10.0.1.1/32;
                address app2 10.0.1.2/32;
                address-set webTier {
                    address web1;
                    address web2;
                }
                address-set appTier {
                    address app1;
                    address app2;
                }
            }
    }
}
```

where 
- "`webTier`" is an alias to 10.0.0.1 and 2.

Above we defined systems based on their IP addresses.  For systems where the IP might change, we can also define them based on their DNS names. 

```
set security address-book global address force.com dns-name force.com ipv4-only
set security address-book global address salesforce.com dns-name salesforce.com ipv4-only
set security address-book global address salesforceliveagent.com dns-name salesforceliveagent.com ipv4-only
set security address-book global address visualforce.com dns-name visualforce.com ipv4-only
set security address-book global address documentforce.com dns-name documentforce.com ipv4-only
set security address-book global address lightning.com dns-name lightning.com ipv4-only
set security address-book global address salesforce-communities.com dns-name salesforce-communities.com ipv4-only
set security address-book global address forceusercontent.com dns-name forceusercontent.com ipv4-only

set security address-book global address-set salesforce address force.com
set security address-book global address-set salesforce address salesforce.com
set security address-book global address-set salesforce address salesforceliveagent.com
set security address-book global address-set salesforce address visualforce.com
set security address-book global address-set salesforce address documentforce.com
set security address-book global address-set salesforce address lightning.com
set security address-book global address-set salesforce address salesforce-communities.com
set security address-book global address-set salesforce address forceusercontent.com
```

## Application Sets: 
Combining current applications into a single one: 

```
set applications application-set webStuff application junos-ping
set applications application-set webStuff application junos-http
set applications application-set webStuff application junos-https
```

or 
```
applications {
    application-set webStuff {
        application junos-ping;
        application junos-http;
    }
}
```

- where the "`junos-`" applications are pre-canned and grouped into a new service called "webStuff"

Creating your own application:

set applications application gate-app protocol tcp
set applications application gate-app source-port 1024-65535
set applications application gate-app destination-port 8443
or 
applications {
    application gate-app {
        protocol tcp;
        source-port 1024-65535;
        destination-port 8443;
    }
}

- where "`gate-app`" is a flow over tcp from ports 1024-65535 to port 8443


## Policy: 
```
set security policies from-zone untrust to-zone newzone policy allowUsers match source-address friendlys
set security policies from-zone untrust to-zone newzone policy allowUsers match destination-address webTier
set security policies from-zone untrust to-zone newzone policy allowUsers match application webStuff
set security policies from-zone untrust to-zone newzone policy allowUsers then permit
set security policies from-zone untrust to-zone newzone policy allowUsers then count
```

and
```
set security policies from-zone newzone to-zone newzone policy backend match source-address webTier
set security policies from-zone newzone to-zone newzone policy backend match destination-address appTier
set security policies from-zone newzone to-zone newzone policy backend match application gate-app
set security policies from-zone newzone to-zone newzone policy backend then permit
set security policies from-zone newzone to-zone newzone policy backend then count
```

or
```
security {
    policies {
        from-zone untrust to-zone newzone {
            policy allowUsers {
                match {
                    source-address friendlys;
                    destination-address webTier;
                    application webStuff;
                }
                then {
                    permit;
                    count;
                }
            }
        }
        from-zone newzone to-zone newzone {
            policy backend {
                match {
                    source-address webTier;
                    destination-address appTier;
                    application gate-app;
                }
                then {
                    permit;
                    count;
                }
            }
        }
    }
}
```

## References: 
- [Junos OS: Address Book and Address Sets for Security Devices (rel.12.1)](https://www.juniper.net/techpubs/en_US/junos12.1/information-products/pathway-pages/security/security-basic-address-book-set.pdf), 6/30/12
- [Troubleshooting SRX security policies](https://www.inetzero.com/troubleshooting-security-policies/):  Richard Pracko, April 2014