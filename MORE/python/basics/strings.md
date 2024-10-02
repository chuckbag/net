# Strings

## Addition vs. Concatenation:
The first adds the two numbers, the second binds the two strings: 
Code:
``` 
one = 1
two = 2
print(one+two)

one = "one"
two = "two"
print(one+two)
```

Output:
```
3
onetwo
```

## Cleaning up strings
Code: 
```
a = 'aa'
b = 'bb'
c = 'cc'
d = 'dd'

u = a, ":", b, ":", c, ":", a

uu =''.join(map(str, u))

print "raw string       ", u
print "cleaned up string ", uu
```
Output: 
```
raw string        ('aa', ':', 'bb', ':', 'cc', ':', 'aa')
cleaned up string  aa:bb:cc:aa
```

