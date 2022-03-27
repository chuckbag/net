# setting network via command line

View all the network interfaces: 

```
$ networksetup -listallhardwareports

Hardware Port: Wi-Fi
Device: en0
Ethernet Address: f4:5c:89:9c:43:d3

Hardware Port: Bluetooth PAN
Device: en3
Ethernet Address: f4:5c:89:9c:43:d4

Hardware Port: Thunderbolt 1
Device: en1
Ethernet Address: 6a:00:01:a1:7b:a0

Hardware Port: Thunderbolt 2
Device: en2
Ethernet Address: 6a:00:01:a1:7b:a1

Hardware Port: Thunderbolt Bridge
Device: bridge0
Ethernet Address: 6a:00:01:a1:7b:a0

VLAN Configurations
===================
```

View the wireless networks available, and their health: 

```
$ /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport scan
                            SSID BSSID             RSSI CHANNEL HT CC SECURITY (auth/unicast/group)
     Virus Detected! Do not use. 58:b6:33:46:61:e8 -59  11      Y  US NONE
                            Woof 58:b6:33:06:61:e8 -59  11      Y  US WPA2(PSK/AES/AES) 
     DIRECT-C2-HP Officejet 5740 50:65:f3:c0:e8:c3 -85  6       Y  -- WPA2(PSK/AES/AES) 
     Virus Detected! Do not use. 58:b6:33:46:67:a8 -46  1       Y  US NONE
                            Woof 58:b6:33:06:67:a8 -44  1       Y  US WPA2(PSK/AES/AES) 
     Virus Detected! Do not use. 58:b6:33:46:61:ec -69  149     Y  US NONE
                            Woof 58:b6:33:06:61:ec -69  149     Y  US WPA2(PSK/AES/AES) 
                            Woof 58:b6:33:06:67:ac -55  157     Y  US WPA2(PSK/AES/AES) 
```

Get information on the current wifi network: 
```
$ /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport -I
     agrCtlRSSI: -64
     agrExtRSSI: 0
    agrCtlNoise: -92
    agrExtNoise: 0
          state: running
        op mode: station 
     lastTxRate: 264
        maxRate: 867
lastAssocStatus: 0
    802.11 auth: open
      link auth: none
          BSSID: 58:b6:33:46:67:ac
           SSID: Virus Detected! Do not use.
            MCS: 6
        channel: 157,80
```

change the wifi network  (and then confirm)
```
$ networksetup -setairportnetwork en0 Woof wifiPassword
$ /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport -I
     agrCtlRSSI: -62
     agrExtRSSI: 0
    agrCtlNoise: -92
    agrExtNoise: 0
          state: running
        op mode: station 
     lastTxRate: 234
        maxRate: 867
lastAssocStatus: 0
    802.11 auth: open
      link auth: wpa2-psk
          BSSID: 58:b6:33:6:67:ac
           SSID: Woof
            MCS: 3
        channel: 157,80
```

