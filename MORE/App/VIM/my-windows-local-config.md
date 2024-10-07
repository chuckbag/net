# My windows local config

## Syntax: 
Standard syntax folder location: 

```
C:\Program Files (x86)\Vim\vim74
```

checked in location: 

```
C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\win\syntax
```

Replace the standard location with a hardlink to the checked in file. 

```
cd "c:Program Files (x86)\vim\vim74"
mklink /d syntax "C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\win\syntax"
```

## Colors: 
Standard colors folder location: 
```
C:\Program Files (x86)\Vim\vim74
```

checked in location: 
```
C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\colors
```

Replace the standard with the checked in version 
```
mklink /d colors "C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\colors"
```

## filetype
Standard filetype file location: 
```
C:\Program Files (x86)\Vim\vim74
```

checked in file location: 
```
C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\win\filetype.vim
```

replace the file with a link to the checked in version.  First delete the file in the program files directory. then run this command: 
```
mklink /h filetype.vim "C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\win\filetype.vim
```

## _vimrc
standard file location: 
```
C:\Users\cmercier\_vimrc
```

checked in location: 
```
C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\win\_vimrc
```

replace the original with the checked one, but first delete the original version, then do the following command: 
```
cd c:\Users\cmercier
del _vimrc
mklink /h _vimrc "C:\Users\cmercier\Documents\home\svnrepo\techops\shared\cmercier\.vim\win\_vimrc"
```
 

## References:
- [Hard Links and Junctions](https://sites.google.com/a/cmed.us/net/home/unixlinux/vim/my-windows-local-config?authuser=1#:~:text=Hard%20Links%20and%20Junctions): Microsoft
- [How to Create and Use Links with MKLINK in Windows](http://www.sevenforums.com/tutorials/278262-mklink-create-use-links-windows.html): 