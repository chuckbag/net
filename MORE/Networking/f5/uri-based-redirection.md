# URI based redirection


# 
In this example, we use the one-armed example (see "one-armed-load-balancing-example.txt) and add the ability to redirect traffic biased on the url of the traffic. Here, we send traffic with the uri of /A* to the testa pool, we send traffic with the uri /B* to the testb pool, etc... 
  
```  
# interfaces 
interface exp0 dest enable 
interface exp0 source enable 
interface exp0 adminport open 
interface exp0 source_translation enable 
interface exp0 mac_masq 4:0:ba:db:ee:f0 
interface exp1 dest disable 
interface exp1 source disable 
interface exp1 adminport open  

# constants 
tping_node 10 timeout_node 31 
tping_svc 80 61 timeout_svc 80 129 
mirror enable gateway failsafe disarm 
snat limit 0 
snat timeout udp 60 
snat timeout tcp 3600  

# ports 
port 80 443 22 enable  

# server pools 
pool opsmainpool 
{    
    lb_method round_robin    
    member 10.10.13.26:http ratio 10 priority 10 
} 
pool testa 
{    
    lb_method round_robin    
    member 10.100.100.10:http ratio 10 priority 10 
} 
pool testb 
{    
    lb_method round_robin    
    member 10.100.100.15:http ratio 10 priority 10 
} 
pool testd 
{    
    lb_method round_robin    
    member 10.100.100.40:http ratio 10 priority 10 
} 
pool teste 
{    
    lb_method round_robin    
    member 10.100.100.45:http ratio 10 priority 10 
}  

# load balancing rules 
rule lab-test-abde 
{    
    if (http_uri starts_with "/A") 
    {       
        use ( testa )    
    }    
    else 
    {       
        if (http_uri starts_with "/B") 
        {          
            use ( testb )       
        }       
        else 
        {          
            if (http_uri starts_with "/D") 
            {             
                use ( testd )          
            }          
            else 
            {             
                if (http_uri starts_with "/E") 
                {                
                    use ( teste )             
                }             
                else 
                {                
                    use ( opsmainpool )             
                }          
            }       
        }    
    } 
}  

# vips 
vip 127.0.0.248:http unit 1 
{     
    netmask 255.255.255.0 broadcast 127.0.0.255     
    use rule lab-test-abde 
}  
    
# snats 
snat map 
{ 
    default to 10.100.100.240 unit 1 netmask 255.255.255.0 
}  

# ratio 
ratio 
{          
    10.10.13.26 10.100.100.10 10.100.100.15 10.100.100.40          
    10.100.100.45    
} 1  

# proxy targets  
proxy 10.100.100.248:https unit 1 
{ 	
    netmask 255.255.255.0 	
    broadcast 10.100.100.255 	
    target vip 127.0.0.248:http 	
    ssl enable 	
    key test2.ops.ariba.com.key 	
    cert test2.ops.ariba.com.cert 
}  

#reaper timeouts 
treaper 80 60 
treaper 443 1005 
treaper 22 1005  

# interface 
failsafe interface exp0 
failsafe disarm interface exp0 timeout 30 interface exp1 
failsafe disarm interface exp1 timeout 30
```

