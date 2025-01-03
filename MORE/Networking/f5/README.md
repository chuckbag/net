# F5

## Configuring the LTM: 
- Upgrading OS: Upgrade to the newer OS
- [Basic Setup](basic-physical-setup-v-11.md): Just getting the interfaces/routes up and working.
- [HA Setup](ha-setup.md): How to link two LB's together

## Basic LB Setups:
- [Basic VIPs](basic-vips.md):  How to enable a simple load balanced pool of servers
- [Stickyness](stickyness.md): Or persistence, or Source Address Affinity Persistence. Making customers traffic stick to one backend server. 
- [SNAT](snat.md): Change the source address of the traffic to handle funky routing


## Troubleshooting: 
- [tcpdump](tcpdump.md): seeing traffic on the LB
- [Network Reset Cause](network-reset-cause.md): If your getting TCP resets, this tells you why
- [iHealth](https://f5.com/support/tools/ihealth?mkt_tok=3RkMMJWWfF9wsRoksqzBc%2B%2FhmjTEU5z16u4lXK%2BxhIkz2EFye%2BLIHETpodcMTstnM77YDBceEJhqyQJxPr3DKdMNydh%2BRhbqCw%3D%3D): F5 tool for checking your configs

## Other: 
- [Old](old.md): Just for the fun of it, old v2-v3 Docs.  