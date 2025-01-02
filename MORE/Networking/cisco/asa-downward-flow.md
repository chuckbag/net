# Downward Flow
Traffic by default, can travel "downward" (from a higher to lower security zone) freely without ACL's.  If you are going from a private IP space to a public, you will need to NAT though (see above).  

## Just NAT

As long as there is no current ACL on the high security interface, an outbound NAT is all that is needed to allow outbound traffic flow.  (you 

```
object network VIP_PAT
  subnet 10.0.0.0 255.0.0.0
  nat (inside,outside) dynamic 5.5.5.5
```

## ACL and outbound NAT: 


