# Accept input from command line statement


Code: 
```
#!/usr/bin/env python
#
# run a command from within the script, and use it's output
#
import subprocess

# run the command "ip addr" and stick the output in "commandOutput"
commandOutput = subprocess.Popen('ip addr', shell=True, stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
#retval = commandOutput.wait()

# print out all the lines of "commandOutput"
for line in commandOutput.stdout.readlines():
    line = line.strip('\n')
    line = line.strip('\r')
    print("> {}".format(line))
```

Output: 
```
cmercier@Balsa ~/bin/python-examples $ ./input-from-command.py
> lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
>  inet 127.0.0.1/8 lo0
>  inet6 ::1/128
>  inet6 fe80::1/64 scopeid 0x1
> en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
>  ether 8c:85:90:58:03:0f
>  inet6 fe80::145b:a2d0:e69c:f037/64 secured scopeid 0x8
>  inet 198.18.2.138/24 brd 198.18.2.255 en0
> awdl0: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1484
>  ether f2:23:7b:ff:47:13
>  inet6 fe80::f023:7bff:feff:4713/64 scopeid 0xe
> utun0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 2000
>  inet6 fe80::97e1:fc90:6cf7:5b96/64 scopeid 0x10
> utun1: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1380
>  inet6 fe80::7d68:8f1f:63ba:137c/64 scopeid 0x11
> en5: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
>  ether ac:de:48:00:11:22
>  inet6 fe80::aede:48ff:fe00:1122/64 scopeid 0x7
> utun2: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1200
>  inet 10.36.32.202 --> 10.36.32.202/32 utun2
```

References: 
- [Subprocess and Shell Commands in Python: Oct, 2013](https://www.pythonforbeginners.com/os/subprocess-for-system-administrators)
