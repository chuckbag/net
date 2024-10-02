# sudo

## Setup root user:
Become root:
```bash
pplnuser@DESKTOP-TU5R9A5:~$ su
Password:
root@DESKTOP-TU5R9A5:/home/pplnuser# 
```

change the default editor:
```bash
root@DESKTOP-TU5R9A5:/home/pplnuser# update-alternatives --config editor
There are 4 choices for the alternative editor (providing /usr/bin/editor).

  Selection    Path                Priority   Status
------------------------------------------------------------
* 0            /bin/nano            40        auto mode
  1            /bin/ed             -100       manual mode
  2            /bin/nano            40        manual mode
  3            /usr/bin/vim.basic   30        manual mode
  4            /usr/bin/vim.tiny    15        manual mode

Press <enter> to keep the current choice[*], or type selection number: 3
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/editor (editor) in manual mode
root@DESKTOP-TU5R9A5:/home/pplnuser#
```

and in the bash settings
```bash
vim ~/.bashrc
```

add the following:
```bash
export VISUAL=vim
export EDITOR="$VISUAL"
```

## Modify sudoers
open the sudoer file
```bash
pplnuser@DESKTOP-TU5R9A5:~$ su
Password:
root@DESKTOP-TU5R9A5:/home/pplnuser# visudo
```

and add the following:
```bash
pplnuser ALL=(ALL) /usr/bin/mount, /usr/bin/umount
```

