# Virginise the box

Always nice to start from a clean slate.  Do the following to remove any old configs on the firewall and start from factory defaults.

```
ASA1# write erase
Erase configuration in flash memory? [confirm]
[OK]
ASA1# reload
System config has been modified. Save? [Y]es/[N]o:
Proceed with reload? [confirm]
[... blablabla ...]
Pre-configure Firewall now through interactive prompts [yes]? n
Type help or '?' for a list of available commands.
ciscoasa>
```
