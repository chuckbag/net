# Script Arguments and Debug



Code: 
```
#!/usr/bin/env python
#
import argparse

# - - - - - - - - - - - - - - -
# Argument Section:
parser = argparse.ArgumentParser(
    description='script that has many levels of debug',
    epilog="output format: <tbd>"
)
parser.add_argument("-d", "--debug", type=int, choices=[1, 2, 3], help="enable debug level")
args=parser.parse_args()

# - - - - - - - - - - - - - - -
# Defining some random variables:
fruit = " I like apples "
debug1 = "simple debug output"
debug3 = " some code that is really detailed "

# - - - - - - - - - - - - - - -
# Do normal program stuff: 
print(fruit)

# - - - - - - - - - - - - - - -
# Show how to debug output:

# Minor debugging:
if args.debug >=1:
 print("> {}".format(debug1))

# Major debugging:
if args.debug >=3: 
    print("  > {}".format(debug3))
```

Output: 
```
chuck@Balsa ~/bin/python-examples $ ./debug-example.py -h
usage: debug-example.py [-h] [-d {1,2,3}]

script that has many levels of debug

optional arguments:
  -h, --help            show this help message and exit
  -d {1,2,3}, --debug {1,2,3}
                        enable debug level

output format: <tbd>
chuck@Balsa ~/bin/python-examples $ ./debug-example.py
 I like apples
chuck@Balsa ~/bin/python-examples $ ./debug-example.py -d 1
 I like apples
> simple debug output
chuck@Balsa ~/bin/python-examples $ ./debug-example.py -d 3
 I like apples
> simple debug output
  >  some code that is really detailed
chuck@Balsa ~/bin/python-examples $
```