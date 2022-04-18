# creating pcaps

- [creating pcaps](#creating-pcaps)
  - [Making the change](#making-the-change)
    - [Firewall filter config](#firewall-filter-config)
    - [Forwarding-options config](#forwarding-options-config)
    - [Interface config](#interface-config)
    - [Enable the capture](#enable-the-capture)
    - [Collecting the captures](#collecting-the-captures)
  - [Reference](#reference)
    - [Reference - Interface](#reference---interface)

## Making the change

### Firewall filter config

In this example, we're wanting to see any traffic in either direction between ports 68 and 67 (which is dhcp traffic).  We create one filter statement "`PCAP-DHCP`" and then have two terms ("`1`" & "`2`"), one for traffic going one direction, and then the second going the other.  Anything that matches the source/destination mapping in those terms is "`accept`"ed, and we also add the "`sample`" statement that tells the firewall to capture the traffic from that match.  The final line is the third term ("`accept-other-traffic`") which allows all other traffic that is going through the firewall.  With out that last line, the firewall would default-deny, and any traffic other then dhcp would be blocked.  
```
set firewall filter PCAP-DHCP term 1 from source-port 68
set firewall filter PCAP-DHCP term 1 from destination-port 67
set firewall filter PCAP-DHCP term 1 then sample
set firewall filter PCAP-DHCP term 1 then accept
!
set firewall filter PCAP-DHCP term 2 from source-port 67
set firewall filter PCAP-DHCP term 2 from destination-port 68
set firewall filter PCAP-DHCP term 2 then sample
set firewall filter PCAP-DHCP term 2 then accept
!
set firewall filter PCAP-DHCP term accept-other-traffic then accept
```

### Forwarding-options config
When the firewall filter starts capturing data, it needs to know what to call the file it stores the data in, and the characteristics of that file (size, etc.). We can define this within the "`forwarding-options packet-capture`" section.  In this example, we are defining the file name to "`DHCP-Capture`" and the max file size "1500".  
```
set forwarding-options packet-capture file filename DHCP-Capture
set forwarding-options packet-capture maximum-capture-size 1500
```

### Interface config
We have defined the filters, but we need to apply it to an interface before it will work.  In this case, we can apply it to irb.66 (vlan 66).   
```
set interfaces irb unit 66 family inet filter input PCAP-DHCP
set interfaces irb unit 66 family inet filter output PCAP-DHCP
```

### Enable the capture 
Simple way to make the change and not have to back it out later, is to run the commit in "`confirmed`" state (will revert if not committed again), in this example, it will roll back the changes in "5" minutes.  
```
# make change
show | compare | no-more
commit check

commit confirmed 5
```

### Collecting the captures
The files will be stored in `/var/tmp/{filename}.{interface_name}`   where the {filename} in this example is "`DHCP-Capture`".  
```bash 
show log /var/tmp/DH?
Possible completions:
  <filename>           Name of log file
  /var/tmp/DHCP-Capture.irb  Size: 16350, Last changed: Aug 11 19:02:57
```

Grab them from your laptop with the following scp command: 
```bash
$ scp 10.36.32.1:/var/tmp/DHCP-Capture.irb ~/tmp/
Password:
DHCP-Capture.irb              100%   16KB  61.4KB/s   00:00
$
```

Please take in mind that capturing and sampling packets is CPU intensive so make sure you don’t leave this running on the firewall. You can try with a “commit confirmed” which will give you 10 minutes to test before all the configuration rollbacks.

## Reference
- [How to create a PCAP packet capture on a J-Series or SRX branch device](https://kb.juniper.net/InfoCenter/index?page=content&id=kb11709): Juniper KB11709, Feb 2018 

### Reference - Interface
As a side reference, defining interfaces via "irb.x" is a bit more unusual then your standard reference to a direct interface.  This would be the configs for an interface like that.  
```
interfaces {
    interface-range prodGroup {
        member ge-0/0/2;
        member ge-0/0/3;
        unit 0 {
            family ethernet-switching {
                interface-mode trunk;
                vlan {
                    members [ prod1 ];
                }
            }
        }
    }
    irb {
        unit 66 {
            description "prod1 - web";
            family inet {
                address 10.36.66.1/24;
            }
        }
    }
}
security{  
  zones {
        security-zone prod1 {
            interfaces {
                irb.66 {
                    host-inbound-traffic {
                        system-services {
                            dhcp;
                            ping;
                        }
                    }
                }
            }
        }
    }
}
vlans {
     description web;
        vlan-id 66;
        l3-interface irb.66;
    }
}
```
 



