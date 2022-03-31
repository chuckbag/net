# Loops



## For Loops: 
Code: 
```
print("print 0 - 4")
for i in range(5):      # for each number i in range 0-4. range(5) function returns list [0, 1, 2, 3, 4]
    print(" ", i)       # this line is executed 5 times. First time i equals 0, then 1, ...

primes = [2, 3, 5, 7]   # create new list
print("new list is ", primes)

for p in primes:        # for each number in "primes" print
    print(" ", p)
```
Output: 
```
print 0 - 4
  0
  1
  2
  3
  4
new list is  [2, 3, 5, 7]
  2
  3
  5
  7
```

## While Loops: 
Code: 
```
square = 0
number = 0

while number < 10:
    square = number ** 2
    print("square of ", number, "is", square)
    number += 1
```
Output: 
```
square of  0 is 0
square of  1 is 1
square of  2 is 4
square of  3 is 9
square of  4 is 16
square of  5 is 25
square of  6 is 36
square of  7 is 49
square of  8 is 64
square of  9 is 81
```


