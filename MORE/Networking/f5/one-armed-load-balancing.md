# One armed load balancing

 This is a very simple one armed load balancing example. 
 
 The only interface that is enabled is the exp1 port.  The exp0 port is disabled an does not have an physical medium connected to it. The vips ip is 10.10.10.147:80, and it redirects the traffic (through the snat 10.10.10.144) to the other web server 10.10.13.165:80.  Since the traffic left the F5 snatted, the web server sends the traffic back to the F5, who then sends the traffic back to the original client. 
   
```   
# interfaces 
interface exp0 dest disable 
interface exp0 source disable 
interface exp0 adminport lockdown 
interface exp0 mac_masq 4:0:ba:db:ee:f0 
interface exp1 dest enable 
interface exp1 source enable 
interface exp1 adminport open 
interface exp1 source_translation enable  

# constants 
tping_node 5 timeout_node 16 
tping_svc 80 5 
tping_svc 443 5 
timeout_svc 80 16 
timeout_svc 443 16 
mirror enable gateway 
failsafe disarm 
snat limit 0 
snat timeout udp 60 
snat timeout tcp 3600  

# ports 
port 80 443 enable  

# server pools 
pool ops_web_example 
{    
    lb_method round_robin
    member 10.10.13.165:http ratio 10 priority 10 
}  


# vips 
vip 10.10.10.147:http unit 1 
{     
    netmask 255.255.254.0 broadcast 10.10.11.255     
    use pool ops_web_example 
}  

# snats 
snat map 
{ 
    default to 10.10.10.144 unit 1 netmask 255.255.254.0 
}  

# ratio 
ratio 
{     
    10.10.13.165    
} 1  

#reaper timeouts 
treaper 80 1005 
treaper 443 1005  

# interface 
failsafe interface exp0 
failsafe disarm interface exp0 timeout 30 interface exp1 
failsafe disarm interface exp1 timeout 30
```
