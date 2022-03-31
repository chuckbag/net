# Accept input from pipe


Code: 
```
#!/usr/bin/env python
import sys

# this script accepts input from a pipe
# example: ls | ./input-from-pipe.py

# accept anything from pipe: 
for line in sys.stdin:

    # clean that input and print it back out: 
    line = line.strip('\n')
    line = line.strip('\r')
    print("in > {}".format(line))
```



Output: 
```
chuck@Balsa ~/bin/python-examples $ ls
debug-example.py*    input-from-pipe.py*  matching-stuff.py*   printing-columns.py*
chuck@Balsa ~/bin/python-examples $
chuck@Balsa ~/bin/python-examples $ ls | ./input-from-pipe.py
in > debug-example.py*
in > input-from-pipe.py*
in > matching-stuff.py*
in > printing-columns.py*
chuck@Balsa ~/bin/python-examples $
```
