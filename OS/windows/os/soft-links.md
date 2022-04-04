# Hard Links

If you are in the directory: `C:\Users\chuck`
and you want a folder called "home" that points to `Documents\home` 

then you would enter in the following command: 
```
mklink /D /H /J home Documents\home
```

```
C:\>mklink 
Creates a symbolic link.

MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      Creates a directory symbolic link.  Default is a file
                symbolic link.
        /H      Creates a hard link instead of a symbolic link.
        /J      Creates a Directory Junction.
        Link    specifies the new symbolic link name.
        Target  specifies the path (relative or absolute) that the new link
                refers to.
```

### References: 
- [Mklink](https://technet.microsoft.com/en-us/library/cc753194(WS.10).aspx): Creates a symbolic link., Applies To: Windows Server 2008, Windows Vista (2012)
