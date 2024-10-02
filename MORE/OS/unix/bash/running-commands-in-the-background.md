# Running commands in the background

Run the command, but get the shell back. End the command with the "&"
```bash
rsync /home/user1/bigfile /home/user2/. &
```
Note that with the above, if you close the terminal, the session will close (unlike a screen session).  


Run the command in the background independent of the terminal.  
If you want to run a command, have it keep running, and log out and have it still run, you need to nohup it. 
```bash
nohup rsync /home/user1/bigfile /home/user2/. & 
```
Any output will be stored in a file called nohup.out.

If you want to do the same as above, but don't want to save the output and send it to dev/null, then do the following: 
```bash
nohup rsync /home/user1/bigfile /home/user2/. &>/dev/null & 
```


## Ref: 
- [Running Bash Commands in the Background the Right Way [Linux]](https://www.maketecheasier.com/run-bash-commands-background-linux/): Derrik Diener, Mar, 2016

