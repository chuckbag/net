# Dictionaries

- [Dictionaries](#dictionaries)
  - [Add/Remove contents to dictionary](#addremove-contents-to-dictionary)
  - [View entire contents, keys or values](#view-entire-contents-keys-or-values)
  - [Checking for matches](#checking-for-matches)
  - [something else](#something-else)
  - [Add string to Dictionary, count dupes](#add-string-to-dictionary-count-dupes)

## Add/Remove contents to dictionary

code
```
# create new dictionary.
phone_book = {"John": 123, "Jane": 234, "Jerard": 345}    # "John", "Jane" and "Jerard" are keys and numbers are values
print("print dictionary = ", phone_book)

# Add new item to the dictionary
phone_book["Jill"] = 345
print("add to dictionary = ", phone_book)

# Remove key-value pair from phone_book
del phone_book['John']
print("lookup value for John = ", phone_book['Jane'])
```
output
```
print dictionary =  {'John': 123, 'Jane': 234, 'Jerard': 345}
add to dictionary =  {'John': 123, 'Jane': 234, 'Jerard': 345, 'Jill': 345}
lookup value for John =  234
```

## View entire contents, keys or values

code
```
phone_book = {"John": 123, "Jane": 234, "Jerard": 345}  # create new dictionary
print("phonebook =    ", phone_book)

# Add new item to the dictionary
phone_book["Jill"] = 456
print("+Jill =        ", phone_book)

print("Just the keys  ", phone_book.keys())

print("just the values", phone_book.values())
```
output
```
phonebook =     {'Jane': 234, 'John': 123, 'Jerard': 345}
+Jill =         {'Jane': 234, 'Jill': 456, 'John': 123, 'Jerard': 345}
Just the keys   dict_keys(['Jane', 'Jill', 'John', 'Jerard'])
just the values dict_values([234, 456, 123, 345])
```

## Checking for matches
You can use the "in" keyword for both lists and dictionaries to check if the list has a matching value.  
code
```
grocery_list = ["fish", "tomato", 'apples']   # create new list

print("tomato is in grocery_list = ", "tomato" in grocery_list)    # check that grocery_list contains "tomato" item

grocery_dict = {"fish": 1, "tomato": 6, 'apples': 3}   # create new dictionary

print("fish is in grocery_list = ", 'fish' in grocery_dict)
print("dog is in grocery list = ", 'dog' in grocery_dict)
```

output
```
tomato is in grocery_list =  True
fish is in grocery_list =  True
dog is in grocery list =  False
```

## something else

Code: 
```
from  collections import OrderedDict

flow = OrderedDict()

# define 1st level of data
a = 'aa'
b = 'bb'
c = 'cc'
d = 'dd'

# combine strings together
u = a, ":", b, ":", c, ":", a
v = a, ":", b, ":", c, ":", d
w = b, ":", b, ":", c, ":", d
x = c, ":", b, ":", c, ":", d
y = d, ":", b, ":", c, ":", d
z = a, ":", b, ":", c, ":", d

# convert array back to a single string
uu =''.join(map(str, u))
vv =''.join(map(str, v))
ww =''.join(map(str, w))
xx =''.join(map(str, x))
yy =''.join(map(str, y))
zz =''.join(map(str, z))

# look at what the array and string looks like
print "raw string       ", u
print "cleaned up string ", uu

# create dictionary
flow[vv] = 1
flow[ww] = 1
flow[xx] = 1
flow[yy] = 1
flow[zz] = 1

# search for keys in the dictionary
for key, value in flow.iteritems():
    if key.startswith(ww):
        print "> found first search: ", key, value

for key, value in flow.iteritems():
    if key.startswith(uu):
        print "> found second search: key, value"

# view the contents of the dictionary
print "\n list of dictionary: ", flow
```
Output: 
```
raw string        ('aa', ':', 'bb', ':', 'cc', ':', 'aa')
cleaned up string  aa:bb:cc:aa
> found first search:  bb:bb:cc:dd 1

 list of dictionary:  OrderedDict([('aa:bb:cc:dd', 1), ('bb:bb:cc:dd', 1), ('cc:bb:cc:dd', 1), ('dd:bb:cc:dd', 1)])
```

## Add string to Dictionary, count dupes
this script creates a couple of strings (the long way), and then puts them into a dictionary (hash).  If the string has already been added, it updates the count for that string.  

Code: 
```
from  collections import OrderedDict

flow = OrderedDict()

# define 1st level of data
a = 'aa'
b = 'bb'
c = 'cc'
d = 'dd'

# combine strings together
u = a, ":", b, ":", c, ":", a
v = a, ":", b, ":", c, ":", d
w = b, ":", b, ":", c, ":", d
x = b, ":", b, ":", c, ":", d
y = d, ":", b, ":", c, ":", d
z = a, ":", b, ":", c, ":", d

# convert array back to a single string
uu =''.join(map(str, u))
vv =''.join(map(str, v))
ww =''.join(map(str, w))
xx =''.join(map(str, x))
yy =''.join(map(str, y))
zz =''.join(map(str, z))

# list of strings
strings = uu, vv, ww, xx, yy, zz
print "list of data: ", strings

# check to see if string exists
for data in strings:
    print "\nNext string data = ", data
    if (data in flow):
        flow[data] += 1
        print "this already is in the string, adding count"
        print "    ", data, " = ", flow[data]
    else:
        flow[data] = 1
        print "new value: "
        print "    ", data, " = ", flow[data]

# view the contents of the dictionary
print "\n list of dictionary: ", flow
```
Output: 
```
list of data:  ('aa:bb:cc:aa', 'aa:bb:cc:dd', 'bb:bb:cc:dd', 'bb:bb:cc:dd', 'dd:bb:cc:dd', 'aa:bb:cc:dd')

Next string data =  aa:bb:cc:aa
new value: 
     aa:bb:cc:aa  =  1

Next string data =  aa:bb:cc:dd
new value: 
     aa:bb:cc:dd  =  1

Next string data =  bb:bb:cc:dd
new value: 
     bb:bb:cc:dd  =  1

Next string data =  bb:bb:cc:dd
this already is in the string, adding count
     bb:bb:cc:dd  =  2

Next string data =  dd:bb:cc:dd
new value: 
     dd:bb:cc:dd  =  1

Next string data =  aa:bb:cc:dd
this already is in the string, adding count
     aa:bb:cc:dd  =  2

 list of dictionary:  OrderedDict([('aa:bb:cc:aa', 1), ('aa:bb:cc:dd', 2), ('bb:bb:cc:dd', 2), ('dd:bb:cc:dd', 1)])
```



