# XForwardFor

## Confirm the Packets are Correct:
View the packets via tcpdump with the following commands:
```bash
tcpdump -nAli <iface> port <port_number> -s 0 | grep 'header_name'
```
or specifically:
```bash
$ tcpdump -nAli eth0 port 80 -s 0 | grep 'X-'
```

you can also use ack instead of grep to see it in color. 

## Setup Apache Logging:
see http://www.techstacks.com/howto/log-client-ip-and-xforwardedfor-ip-in-apache.html

