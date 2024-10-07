# FIPS 140-2 Compliant

Few other changes to make to add a bit of extra security to the firewall. 

```
conf t
!
mode single
no firewall transparent
crashinfo console disable
fips enable
no service password-recovery
config-register 0x1001
!
end
```

Where :
- `mode single`: prevents you from running systems in virtual mode, and having more then one fw in the chassis.
- `no service password-recovery`: makes it so that when you do a password recovery, it automatically blows away the current running config.  (so that no one else gets to see what it is.)

