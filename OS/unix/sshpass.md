# sshpass

## Overview
If you want to script out a bunch of ssh commands, when you run that script you'll end up needing to type in the password each time your script runs an ssh command.  This can be completely insane and immediately ruin the point of the script.  

sshpass comes to the rescue by allowing you to store your password in a file locally, and then when you run ssh commands, it automatically enters in the password without user prompt.  (the only problem is that you *really* should not store password files locally.  So look into wrapping this all in a gpg wrapper.)

If possible, do not use this, but instead write your scripts using ansible!  

## Password File
sshpass needs to know what the password should be that it will pass to ssh.  

Create a local file "xxx" with the root password in it.

### Local ssh commands
Run a local command that prompts for a password, but don't get prompted for the password.  

In this example, we can run a local rsync command, but not get prompted for the password for user "foo".  
```
sshpass -f xxx rsync --progress -ave ssh git/ConfigServer3/deployment foo@10.36.64.110:/tmp/.
```

## Commands on Remote Hosts
We can run commands on remote hosts as well.  

Here, we're running the command "updateIptables.sh" on host 10.36.64.110, by the user "bar".  And all this is being done without asking for a password, because that's stored in the local file "xxx".
```
sshpass -f xxx ssh bar@10.36.64.110 '/tmp/deployment/scripts/updateIptables.sh'
```
