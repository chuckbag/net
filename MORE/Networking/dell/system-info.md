# system info

view model and os: 
```
os-swa1n01>sh ver
Dell Real Time Operating System Software
Dell Operating System Version:  2.0
Dell Application Software Version:  9.10(0.1)
Copyright (c) 1999-2016 by Dell Inc. All Rights Reserved.
Build Time: Wed May 11 23:07:56 2016
Build Path: /sites/eqx/work/swbuild01_1/build08/E9-10-0/SW/SRC
Dell Networking OS uptime is 3 minute(s)
System image file is "system://A"
System Type: S4048-ON 
Control Processor: Intel Rangeley with 3 Gbytes (3201302528 bytes) of memory, core(s) 2.
8G bytes of boot flash memory.
  1 54-port TE/FG (SK-ON)
 48 Ten GigabitEthernet/IEEE 802.3 interface(s)
  6 Forty GigabitEthernet/IEEE 802.3 interface(s)
bos-swa1n01>
```

base mac, 
```
bos-swa1n01>sh system 
Stack MAC                  : f4:8e:38:55:24:f6
Reload-Type                         : normal-reload [Next boot : normal-reload]
--  Unit 1 --
Unit Type                  : Management Unit
Status                     : online 
Next Boot                  : online 
Required Type              : S4048-ON - 54-port TE/FG (SK-ON)
Current Type               : S4048-ON - 54-port TE/FG (SK-ON)
Master priority            : 0 
Hardware Rev               : 2.0 
Num Ports                  : 72 
Up Time                    : 6 min, 48 sec
Dell Networking OS Version : 9.10(0.1) 
Jumbo Capable              : yes 
POE Capable                : no 
FIPS Mode                  : disabled 
Burned In MAC              : f4:8e:38:55:24:f6 
No Of MACs                 : 3
--  Power Supplies  --
Unit   Bay   Status       Type    FanStatus   FanSpeed(rpm)
---------------------------------------------------------------------------
  1     1    down         UNKNOWN down        0
  1     2    up           AC      up          10320
--  Fan  Status  --
Unit Bay   TrayStatus  Fan1    Speed   Fan2    Speed   
------------------------------------------------------------------------------------
 1    1     up          up      10031   up      10031   
 1    2     up          up      19660   up      19660   
 1    3     up          up      20062   up      20062   
Speed in RPM
bos-swa1n01>
```

see serial number, os, and model number
```
bos-swa1n01>sh inv
System Type            : S4048-ON   
System Mode            : 1.0       
Software Version       : 9.10(0.1)
Unit Type               Serial Number  Part Number  Rev  Piece Part ID            Rev  Svc Tag  Exprs Svc Code
--------------------------------------------------------------------------------------------------------------
* 1  S4048-ON-01-FE-72  NA             0TF3V9       A01  CN-0TF3V9-28298-6AF-0014 A01  4VYXWC2  106 402 692 98
  1  S4048-ON-PWR-AC    NA             0KTM6X       A00  CN-0KTM6X-28298-6AF-0238 A00  NA       NA            
  1  S4048-ON-FAN       NA             06MY5N       A00  CN-06MY5N-28298-6AF-0710 A00  NA       NA            
  1  S4048-ON-FAN       NA             06MY5N       A00  CN-06MY5N-28298-6AF-0709 A00  NA       NA            
  1  S4048-ON-FAN       NA             06MY5N       A00  CN-06MY5N-28298-6AF-0708 A00  NA       NA            
 * - Management Unit 
 
Software Protocol Configured 
--------------------------------------------------------------
  LLDP 
  PVST 
  SNMP 
 
bos-swa1n01>
```
