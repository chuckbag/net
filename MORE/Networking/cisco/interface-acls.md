# Interface ACL's
To make an acl work, the least you need to create is a rule, and bind it to an interface.  

```
object-group network SERVERS
  des --- list of servers --
  network-object host 10.33.3.10
  network-object host 10.33.3.20
  network-object host 10.33.3.30
!
object-group service SERVERPORTS
  des --- ports opened on servers ---
  service-object tcp destination eq http
  service-object tcp destination eq ftp
  service-object tcp destination eq smtp
!
access-list EXTIN extended permit object-group SERVERPORTS any object-group SERVERS
!
access-group EXTIN in interface DMZ1
access-group EXTIN in interface DMZ2
```
