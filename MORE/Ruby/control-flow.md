# Control Flow

If
```
if 2>1
 print "true"
end
```
else
```
if 2>4
 print "true"
else
 print false
end
```
elsif

```
if 2>2
 print "true"
elsif 2<2
 print "false"
else
 print "same"
end
```
unless
```
hungry = false
unless hungry
  puts "I'm writing Ruby programs!"
else
  puts "Time to eat!"
end
```
Equal or Not?
```
is_true = 2 != 3 
is_false = 2 == 3
```
Less than or Grater than
```
test_1 = 17 > 16
test_2 = 21 < 30
test_3 = 9 >= 9
test_4 = -11 <= 4
```
And
```
# boolean_1 = 77 < 78 && 77 < 77
boolean_1 = false

# boolean_2 = true && 100 >= 100
boolean_2 = true
```

Or
```
# boolean_1 = 2**3 != 3**2 || true
boolean_1 = true

# boolean_2 = false || -10 > -9
boolean_2 = false
```

Not
```
# boolean_1 = !true
boolean_1 = false

# boolean_2 = !true && !true
boolean_2 = false
```

Combining Boolean Operators
```
# boolean_1 = (3 < 4 || false) && (false || true)
boolean_1 = true

# boolean_2 = !true && (!true || 100 != 5**2)
boolean_2 = false

```
