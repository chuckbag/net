# "Grep" Findstr

Look for all opened connections to or from a box with the address 10.33.*.*: 
```
C:\Users\crx>netstat -a | findstr "10.33"
  TCP    10.33.32.199:139       win01:0                LISTENING
  TCP    10.33.32.199:3389      10.50.34.185:61271     ESTABLISHED
  TCP    10.33.32.199:49173     WIN7SERVER:microsoft-ds  ESTABLISHED
  UDP    10.33.32.199:137       *:*
  UDP    10.33.32.199:138       *:*
  UDP    10.33.32.199:1900      *:*
  UDP    10.33.32.199:60866     *:*

C:\Users\crx>
```

## References: 
- [Findstr - TechNet - Microsoft](http://technet.microsoft.com/en-us/library/bb490907.aspx):
- [Microsoft DOS findstr command examples](http://www.computerhope.com/findstr.htm): 
 