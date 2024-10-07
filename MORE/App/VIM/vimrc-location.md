# vimrc-location

## Finding your config file location: 

### Unix
In unix systems, it's simple, it's in your home directory and it's named 
```
.vimrc
```

### Windows 
On windows boxes, it might be a bit more complex.  Easiest way to find out is within vm type: 
```
:echo $HOME
```

Normally it will be located in your `C:\Users\<username>` directory.  

I like to change the path into an SVN repo.  To do this, I use symlinks: 
``` shell
mklink /h  c:\Users\chuck\_vimrc  c:\Users\chuck\Documents\home\svnrepo\shared\chuck\.vimrc
mklink /h  c:\Users\chuck\_gvimrc c:\Users\chuck\Documents\home\svnrepo\shared\chuck\.vimrc
mklink /d  c:\Users\chuck\.vim    c:\Users\chuck\Documents\home\svnrepo\shared\chuck\.vim
```

## File Naming Standard: 

### Unix: 
The file starts with a dot to indicate that it is a hidden file.  
```
.vimrc
```

### Windows: 
The file starts with a underscore since windows bites. (maybe that's not helpful... whatever)
```
_vimrc
```
