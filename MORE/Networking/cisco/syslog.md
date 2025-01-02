# Syslog

## Overview:

## Enabling Logging:

```
logging host 10.50.32.65 transport tcp port 5150
logging host 10.50.32.66 transport tcp port 5150
logg source-interface vlan 950
logg trap notifications
logg on
```

## References:
- [Cisco IOS Release 12.0 Configuration Fundamentals Configuration Guide](http://www.cisco.com/en/US/customer/docs/ios/12_0/configfun/configuration/guide/fctroubl.html#wp4901): Cisco Systems, Inc.      
- [Embedded Syslog Manager (ESM)](http://www.cisco.com/en/US/docs/ios/12_3t/12_3t2/feature/guide/gt_esm.html): Cisco IOS Software Releases 12.3 T

