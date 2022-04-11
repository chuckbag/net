# Running with specific users


Normally we run an ansible script with ssl keys setup and with user rights so you can simply run the plays/roles and not have to worry about it.  

But if you want to run this from another host, you might have to enter in both what user the play needs to run as, and also define the password for that user.  

## At the command prompt: 
to set the user that this play will run as, use the attribute `--user root`, and to manually enter the password for that user use the attribute `--ask-pass`. 
```bash
$ ansible-playbook -i hostlist-dga.txt play-getInventory.yml --user root --ask-pass
SSH password:

PLAY [all] *********************************************************************************************************
[...]
```

## In the code

## Reference: 
