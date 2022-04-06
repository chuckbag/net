# Modifying the bashrc file

## Overview: 
bash is a command shell in unix.  You can modify the shell a lot to make it look just the way you want.  The following is my marked up bash shell setup. 

### In Linux:
create the following file: 
```bash
vim .bashrc
```

### On a Mac: 
create the following file: 
```bash
vim .bash_profile
```

### Contents of the file:


In the file, add the following: 
```bash
# .bashrc
export EDITOR=/usr/bin/vim
export PS1='\[\e[1;36m\][SBO]\[\e[0;32m\]\u\[\e[m\]@\h \[\e[1;34m\]\w\[\e[m\] \[\e[1;32m\]\$\[\e[m\] \[\e[1;37m\]'


# see: https://wiki.archlinux.org/index.php/Color_Bash_Prompt

#===============================================================
#
# ALIASES AND FUNCTIONS

#-------------------
# Personal Aliases
#-------------------

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
# -> Prevents accidentally clobbering files.
alias mkdir='mkdir -p'

alias h='history'
alias j='jobs -l'
alias which='type -a'
alias ..='cd ..'
alias path='echo -e ${PATH//:/\\n}'
alias libpath='echo -e ${LD_LIBRARY_PATH//:/\\n}'
alias print='/usr/bin/lp -o nobanner -d $LPDEST'
            # Assumes LPDEST is defined (default printer)

alias du='du -kh'       # Makes a more readable output.
alias df='df -kTh'

#-------------------------------------------------------------
# The 'ls' family (this assumes you use a recent GNU ls)
#-------------------------------------------------------------
alias ll='ls -lh --color'
alias ls='ls -hFh --color'  # add colors for filetype recognition
alias la='ls -Alh'          # show hidden files
alias lx='ls -lXB'         # sort by extension
alias lk='ls -lSr'         # sort by size, biggest last
alias lc='ls -ltcrh'        # sort by and show change time, most recent last
alias lu='ls -ltur'        # sort by and show access time, most recent last
alias lt='ls -ltr'         # sort by date, most recent last
alias lm='ls -al |more'    # pipe through 'more'
alias lr='ls -lRh'          # recursive ls
alias tree='tree -Csu'     # nice alternative to 'recursive ls'

#-------------------------------------------------------------
# the colors to use for the ls command:
#-------------------------------------------------------------
LS_COLORS='di=1;93:fi=0:ln=31:pi=5:so=5:bd=5:cd=5:or=31:mi=0:ex=35:*.rpm=90'
export LS_COLORS

# where:
# di = directory
# fi = file
# ln = symbolic link
# pi = fifo file
# so = socket file
# bd = block (buffered) special file
# cd = character (unbuffered) special file
# or = symbolic link pointing to a non-existent file (orphan)
# mi = non-existent file pointed to by a symbolic link (visible when you type ls -l)
# ex = file which is executable (ie. has 'x' set in permissions).
#
# The *.rpm=90 parameter at the end tells ls to display any files ending in .rpm in the specified colour
#
# 0   = default colour
# 1   = bold
# 4   = underlined
# 5   = flashing text
# 7   = reverse field
# 31  = red
# 32  = green
# 33  = orange
# 34  = blue
# 35  = purple
# 36  = cyan
# 37  = grey
# 40  = black background
# 41  = red background
# 42  = green background
# 43  = orange background
# 44  = blue background
# 45  = purple background
# 46  = cyan background
# 47  = grey background
# 90  = dark grey
# 91  = light red
# 92  = light green
# 93  = yellow
# 94  = light blue
# 95  = light purple
# 96  = turquoise
# 100 = dark grey background
# 101 = light red background
# 102 = light green background
# 103 = yellow background
# 104 = light blue background
# 105 = light purple background
# 106 = turquoise background
#
# These can even be combined, so that a parameter like:
# di=5;31;42
#
```
