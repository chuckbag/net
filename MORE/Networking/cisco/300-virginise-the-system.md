# Virginise the System
Erase the startup config file, and reload the box:

```
#delete startup-config

Delete startup-config [y/n]? y
1#30-Aug-2011 11:12:07 %FILE-I-DELETE: File Delete - file URL flash://startup-config
#reload
You haven't saved your changes. Are you sure you want to continue ? (Y/N)[N] Y
This command will reset the whole system and disconnect your current session. 
Do you want to continue ? (Y/N)[N] Y
Shutting down ...
```

Note that you can also do this by changing the system mode (see above)

If you are using the gui, you can bring the config back to the factory defaults via the left menu bar's Administration, Reboot tabs, and then select the button "Reset to Factory Defaults". 