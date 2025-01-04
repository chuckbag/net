# config accounts
Go into the user section
```
admin > configuration
admin > Config > users
admin > Config > User >
```

add a group called dork, and set it as group operator, and allow access to all the TCP ports
```
admin > Config > User > addgroup name dork sharing true ports * class op
Adding group...
Group techops : Configuration Saved
```

define the user, assign it to a group, and give it a password.  
```
admin > Config > User > adduser user dork fullname silly_user group tech password changeme active true
User techops : Configuration Saved
```

