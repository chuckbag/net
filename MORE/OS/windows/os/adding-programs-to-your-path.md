# adding programs to your PATH

Sometimes you need to add a app to your path:

<img src="../img/path1.png">

To do this (in Win7), go to the START button, select CONTROL PANEL, and select the SYSTEM icon.

Select the Advanced system settings button

<img src="../img/path2.png">

and then under the Advanced tab, select Environment Variables...

<img src="../img/path3.png">

Under User variables for <username>, select PATH, and select Edit..

<img src="../img/path4.png">

At the end of the Variable value: field, add the new entry to the path.  (make sure you separate it with the previous statement with a semicolon.  )
```
;C:\Program Files (x86)\GnuWin32\bin
```

Open up a new shell and confirm it works:

<img src="../img/path5.png">


