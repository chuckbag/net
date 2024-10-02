# Signal Loss


DB loss and real world examples.  

| Signal Strength	| Description	| Required for| 
|--|--|--|
-30 dBm	|Max achievable signal strength. The client can only be a few feet from the AP to achieve this. Not typical or desirable in the real world.	|N/A
-67 dBm	|Minimum signal strength for applications that require very reliable, timely packet delivery.	|VoIP/VoWiFi, streaming video
-70 dBm	|Minimum signal strength for reliable packet delivery.|	Email, web
-80 dBm	|Minimum signal strength for basic connectivity. Packet delivery may be unreliable.|	N/A
-90 dBm	|Approaching or drowning in the noise floor. Any functionality is highly unlikely.|	N/A

Chart listing some materials and approximate DB loss when transmitting through them. 

|Material	|DB loss|
|--|--|
Plasterboard wall	|3dB
Glass wall with metal frame	|6dB
Cinder block wall	|4dB
Office window	|3dB
Metal door	|6dB
Metal door in brick wall	|12.4dB


## References: 
- [Indoor Wireless Pass Loss](https://security-today.com/articles/2012/04/01/indoor-wireless-path-loss.aspx): 
- [WiFi Transmit Power Calculations](http://www.digitalairwireless.com/wireless-blog/t-eirp/wifi-transmit-power-calculations-made-simples.html): 