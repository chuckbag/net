# screen

## Overview: 
Screen is a great tool to use when working on a ssh console.  It enables you to not have to worry when your remote session is dropped because of a funky wifi or wan link.  It's also nice because you can easily have multiple named windows within the single ssh console, and you can easily log the output to a file.  (IE: setup a long compile on one window, and log the output to a file, then open up another and get some work done.  If your session gets hung, dial back in and reconnect with the remote sessions not knowing any better.)
- [Official Screen Home](http://www.gnu.org/software/screen/)
- [Man Page](http://www.manpagez.com/man/1/screen/) 

## Things you can do do within screen: 



|Command	| What it does|
|--|--|
screen |  start screen
`[Crtl]`+`a` `A`<br>{then enter a name}	|  name the session
`[Ctrl]`+`a` `[Exc]`<br>{then `[Page Up]` or `[Page Down]` as often as you want}	 | page up or down in your screen window
`[Crtl]`+`a` `d` |  detach from session while keeping it running (maybe a compile is going on?)
`screen -D -R` |  re-attach (go back) to the original screen:  You can use this to reestablish a screen connection if your session gets dropped.  (think vpn session is dropped)
`[Crtl]`+`a` `c` |  quickly start another session
`[Crtl]`+`a` `A` |  name that window
`[Ctrl]`+`a` `"` |  get a list of all the windows, and flip between them
`[Ctrl]`+`a` `k` |  shut down the open window
`[Ctrl]`+`a` `[Tab]` `e` |  tab between sessions
`[Ctrl]`+`a` `#`<br> {where # is 0,1,2...9}	 | goto 1st (0), 2nd (1), etc screen window
`[Ctrl]`+`a` `H` |  start/stop logging of everything from the screen to file "hardcopy.n"
`[Ctrl]`+`a` `[Ctrl]`+`\` |  quit screen and shutdown all windows


From outside of screen you can 

|Command	| What it does|
|--|--|
`screen -ls` |  list all the active screen sessions
`screen -r` {name} |  reattach to a specific screen.  You can enter a unique first letter or two for the screen name, you don't need to type the entire thing.
`screen -S` <name> |  create a new screen session and name it 
`screen -d -r` <name> |  Reattach to a screen session that is already attached.  




