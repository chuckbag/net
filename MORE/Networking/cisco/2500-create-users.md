# Create Users

Change the password of the admin user: 
```
config mgmtuser password admin {admin_password_in_plaintext}
```

Add a new admin user: 
```
config mgmtuser add chuck {admin_password_in_plaintext} read-write
```
