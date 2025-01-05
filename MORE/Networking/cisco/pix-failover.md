# Upgrading PIX OS in a failover environment

This note is added in response to Cisco's pathetic email in which they told us that there was no way to upgrade two pixes that are setup in failover, without down time.

The way I came up with, still has downtime, but is much easer to pull off.

The assumptions for this is that you are running ios code higher then 5.1(0), that your tftp servers ip address is 172.22.1.202, and that the new ios code is located in the root of the tftp server and labeled "pix525.bin".

Following is the steps needed to take:

( NOTE: Make sure that you have a console window up for both pixes! You are going to have to work quick for steps 2 & 3, so you want to see what the pixes are doing at the console.)

Step| 	On Pix 1 (primary)| 	On Pix 2 (secondary) | 	Notes
---|---|---|---
1 | `sh ver` <br> `sh fail` | `sh ver` <br>`sh fail` | Make sure you have the correct os running, and make sure that you know for sure which pix is primary and which is secondary.
2 | `ping {some outside host}` | `ping {some outside host}` | Sanity check. Make sure that you can ping a host in front of the pix and behind. (this will be repeated on step 4)
3 | `copy tftp://172.22.1.202/pix525.bin flash` | `copy tftp://172.22.1.202/pix525.bin flash` | This adds the new ios onto the pixes, without reloading the box! So you can do this whenever, and will not see a problem with the pix rebooting or anything. The new code will be in effect as soon as you reboot the pix.
4 | `reload` | | Reload the primary pix. When it comes up, just after the rmon timeout, then quickly go to step 3
5 | | | `reload` | This will reload the secondary pix. Note that in between these two steps is a few seconds of downtime. When the box comes back up, make sure that it responds with a Sync Complete, stating that it linked with the primary and downloaded the correct configs.
6 | `ping {some outside host}`<br> `sh ver`<br> `sh fail` | `ping {some outside host}`<br> `sh ver`<br> `sh fail` | Make sure that the new os took, and more importantly that the two boxes are in failover. Also, the ping makes sure that the switch cam's and the hosts all have the correct mac address's and stuff.


## References
- [Upgrading PIX Firewall Software](http://www.cisco.com/univercd/cc/td/doc/product/iaabu/pix/pix_61/config/upgrade.htm#28099):Upgrading Failover Systems
- [PIX 515 Configuration](http://www.cisco.com/univercd/cc/td/doc/product/iaabu/pix/pix_v51/config/bootmode.htm#38962):copy tftp flash Command
- [Advanced Configurations](http://www.cisco.com/univercd/cc/td/doc/product/iaabu/pix/pix_60/config/advanced.htm#xtocid3446):Failover
