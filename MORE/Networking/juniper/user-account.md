# User Account

## Add a user account: 
when creating a user, you define their username, you note their full name, set their userid and class, and then provide a password: 

```
set system login user chuck full-name "chuckiecheezie"
set system login user chuck uid 2001
set system login user chuck class superuser
set system login user chuck authentication plain-text-password
{enter password}
```

When you do a show, you'll see the change comes out looking like this: 
```c
system {
    login {
        user chuck {
            full-name "chuckiecheezie";
            uid 2001;
            class superuser;
            authentication {
                encrypted-password "a5kjAXW7GKhea5kjAXW7GKhea5kjAXW7GKhe"; ## SECRET-DATA
            }
        }
    }
}
```

Note that you will need to make sure that the interface that you want to connect to has the proper `security-zone` setting, and allows ssh for `host-inbound-traffic system-services`.  


