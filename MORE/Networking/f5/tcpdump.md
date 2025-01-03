# tcpdump

The -s0 flag provides a little more data, like what vip (lis) is receiving the traffic. 
```
[dude@lb1n01:Active:In Sync] ~ # tcpdump -s0 -ni n1-trans1 host 10.120.65.32 and port 80
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on n1-trans1, link-type EN10MB (Ethernet), capture size 65535 bytes
23:12:33.263794 IP 10.50.34.204.61050 > 10.120.65.32.http: S 2166173138:2166173138(0) win 8192 <mss 1350,nop,wscale 8,nop,nop,sackOK> in slot1/tmm0 lis=
23:12:33.263838 IP 10.120.65.32.http > 10.50.34.204.61050: R 0:0(0) ack 2166173139 win 0 out slot1/tmm0 lis=/Common/www.ecb.europa.eu
23:12:33.513458 IP 10.50.34.204.61051 > 10.120.65.32.http: S 3036920454:3036920454(0) win 8192 <mss 1350,nop,wscale 8,nop,nop,sackOK> in slot1/tmm1 lis=
23:12:33.513511 IP 10.120.65.32.http > 10.50.34.204.61051: R 0:0(0) ack 3036920455 win 0 out slot1/tmm1 lis=/Common/www.ecb.europa.eu
23:12:33.884521 IP 10.50.34.204.61050 > 10.120.65.32.http: S 2405598283:2405598283(0) win 8192 <mss 1350,nop,wscale 8,nop,nop,sackOK> in slot1/tmm0 lis=
23:12:33.884587 IP 10.120.65.32.http > 10.50.34.204.61050: R 0:0(0) ack 239425146 win 0 out slot1/tmm0 lis=/Common/www.ecb.europa.eu
23:12:34.121479 IP 10.50.34.204.61051 > 10.120.65.32.http: S 1098423313:1098423313(0) win 8192 <mss 1350,nop,wscale 8,nop,nop,sackOK> in slot1/tmm1 lis=
23:12:34.121533 IP 10.120.65.32.http > 10.50.34.204.61051: R 0:0(0) ack 2356470156 win 0 out slot1/tmm1 lis=/Common/www.ecb.europa.eu
23:12:34.492852 IP 10.50.34.204.61050 > 10.120.65.32.http: S 2321051575:2321051575(0) win 8192 <mss 1350,nop,nop,sackOK> in slot1/tmm0 lis=
23:12:34.492907 IP 10.120.65.32.http > 10.50.34.204.61050: R 0:0(0) ack 154878438 win 0 out slot1/tmm0 lis=/Common/www.ecb.europa.eu
```

Note the highlighted "R", this says that the F5 is sending back a TCP reset.  