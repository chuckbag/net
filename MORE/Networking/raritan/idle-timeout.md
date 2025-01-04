# idle timeout
A low idle timeout is good to prevent you from being locked out of a session if your network connection drops unexpectedly.  But ff you are setup multiwrite, where multiple users can see the same terminal, then idle timeout is not that important.  

To increase the time before an unused session is torn down, make the following changes (in this case, setting it to 60 minutes): 

```
admin >
admin > security
admin > Security >
admin > Security > loginsettings
admin > Security > LoginSettings > idletimeout time 60
```

To view the settings do the following 
```
admin >
admin > security
admin > Security >
admin > Security > loginsettings
admin > Security > LoginSettings > idletimeout
Idle Timeout Settings :
        Enable : true
        Timeout : 60 min
admin > Security > LoginSettings >
```

