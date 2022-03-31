# Lists
lists of variables that can be added to and replaced. 

Code: 
```
animals = ['a', 'b', "c", "d", "e", 'f']   # create new list
print("initial string =               ", animals)

animals[1:3] = ['B']    # replace 2 items -- 'b' and 'c' with one item -- 'B'
print("replace b & c with B =         ", animals)

animals[1:3] = []     # remove 2 items -- 'B' and 'd' from the list
print("replace B & d with nothing =   ", animals)

animals += ["1", '2']    # add two items to the list
print("add two items to the list =    ", animals)

animals.append("3")   # add one more item to the list using append() method
print("add one more thing to the list ", animals)

animals[4] = "position-4"
print("replace 2 =                    ", animals)

animals.clear()
print("clear the list =               ", animals)
```
Output: 
```
initial string =                ['a', 'b', 'c', 'd', 'e', 'f']
replace b & c with B =          ['a', 'B', 'd', 'e', 'f']
replace B & d with nothing =    ['a', 'e', 'f']
add two items to the list =     ['a', 'e', 'f', '1', '2']
add one more thing to the list  ['a', 'e', 'f', '1', '2', '3']
replace 2 =                     ['a', 'e', 'f', '1', 'position-4', '3']
clear the list =                []
```

