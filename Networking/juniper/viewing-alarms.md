# Viewing alarms


## See what the current alarms are
```bash
cm@fw1b01> show system alarms
2 alarms currently active
Alarm time               Class  Description
2018-03-08 04:00:33 UTC  Minor  Autorecovery information needs to be saved
2018-03-08 04:00:33 UTC  Minor  Rescue configuration is not set
```


## Clearing AutoRecovery Alarms
Set the Rescue config: 
```bash
cm@fw1b01> request system configuration rescue save

cm@fw1b01>
```

Confirm: 
```bash
cm@fw1b01> show system alarms
1 alarms currently active
Alarm time               Class  Description
2018-03-08 04:00:33 UTC  Minor  Autorecovery information needs to be saved

cm@fw1b01>
```

Save the Autorecovery info: 
```bash
cm@fw1b01> request system autorecovery state save
Saving config recovery information
Saving license recovery information
Saving BSD label recovery information

cm@fw1b01>
```


Confirm: 
```bash
cm@fw1b01> show system alarms
No alarms currently active

cm@fw1b01>
```


## References 
- [What that orange alarm LED light in a Juniper SRX router is trying to tell me?](http://unixwars.blogspot.com/2014/04/what-that-orange-alarm-led-light-in.html): Mauricio Tavares, April 2014
- [SRX210 Services Gateway LEDs](https://www.juniper.net/documentation/en_US/release-independent/junos/topics/concept/leds-srx210.html): Juniper Support, Sep 2017
