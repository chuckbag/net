# Print output in columns
- [Print output in columns](#print-output-in-columns)
  - [Method 1:](#method-1)
  - [Method 2:](#method-2)
  - [Method 3:](#method-3)
  - [Method 4:](#method-4)

## Method 1: 

Code 1: 
```
#!/usr/bin/env python

# set the variables: 
a="what"
b="qty"
c="price"

A="apples"
B="5"
C="$2.00"

# print output
print("%-15s %-15s %s" %("row1", "row2", "row3"))
print("%-15s %-15s %s" %(a, b, c))
print("%-15s %-15s %s" %(A, B, C))
```
Output 1: 
```
$ ./printing-columns.py
row1            row2            row3
what            qty             price
apples          5               $2.00
```

## Method 2: 

Code 2: 
```
#!/usr/bin/env python

# set the variables
a="what"
b="qty"
c="price"

A="apples"
B="5"
C="$2.00"

# print output
for args in (
        ('apple', '$1.09', '80'),
    ):

    print '{0:<10} {1:>8} {2:>8}'.format(*args)
```

Output 2: 
```
$ ./printing-columns.py
apple         $1.09       80
```

## Method 3: 

Code 3: 
```
#!/usr/bin/env python

# set the variables
a="what"
b="qty"
c="price"

A="apples"
B="5"
C="$2.00"

# print output
print('{:<20}'.format('hello'))
print('{:<8} {:<8} {:<8}'.format(a,b,c))
print('{:<8} {:<8} {:<8}'.format(A,B,C))
```

Output 3: 
```
./printing-columns.py
hello
what     qty      price
apples   5        $2.00
```

## Method 4: 
Install Module: 
```
python -m pip install PrettyTable
```
Code: 
```
#!/usr/bin/env python
from prettytable 
import PrettyTable


t = PrettyTable(['Name', 'Age'])
t.add_row(['Alice', 24])
t.add_row(['Bob', 19])
print(t)
```
Output: 
```
$ ./PrettyTable.py
+-------+-----+
|  Name | Age |
+-------+-----+
| Alice |  24 |
|  Bob  |  19 |
+-------+-----+
```
