# Looping

While Loop
```
counter = 1
while counter < 11
  puts counter
  counter = counter + 1
end
```

Danger: Infinite Loops!
```
i = 0  # example of loops that you need to be cautious of
while i < 5
  puts i
  i=i+1
end
```

Until Loop
```
counter = 1
until counter > 10
  puts counter
  counter = counter+1
end
```

Assignment Operators
```
counter = 1
while counter < 11
  puts counter
  counter += 1 # adds 1 to counter
               # could also use -= *= /=
end
```

For loop
```
for num in 1...10  # 3 dots are from 1 to 9
  puts num
end
```

Inclusive and Exclusive ranges
```
for num in 1..15  # two dots are from 1 to 15
  puts num
end
```

The loop Method
```
i = 20
loop {
  i -= 1
  print "#{i} " # prints 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
  break if i <= 0
}
```

Next!
```
i = 20
loop {
  i -= 1
  next if i % 2 != 0 # prints 18 16 14 12 10 8 6 4 2 0
  print "#{i} "
  break if i <= 0
}
```

Saving Multiple Values
```
my_array = [1,2,3,4,5] # array (stored the same as a scalar)

the .each iterator
array = [1,2,3,4,5]

array.each { |x|  # for each "x" do the following
  x += 10
  print "#{x}"    # prints 11 12 13 14 15
}
```

the .times iterator
```
3.times {print "booga "} #repeat this 3 times.  "booga booga booga"
```



