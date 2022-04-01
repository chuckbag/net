# Blocks and Sorting


Why Methods?  (Subroutines)
```
def prime(n)
  puts "That's not an integer." unless n.is_a? Integer
  is_prime = true
  for i in 2..n-1
    if n % i == 0
      is_prime = false
    end
  end
  if is_prime
    puts "#{n} is prime!"
  else
    puts "#{n} is not prime."
  end
end

prime(2)
prime(9)
prime(11)
prime(51)
prime(97)
 # prints 2 is prime!
 #        9 is not prime.
 #        11 is prime!
 #        51 is not prime.
 #        97 is prime!
```

Method Syntax
```
def count  # start with "def"
  (1..10).each { |i| print "#{i} " }
  print "\n"
end        # end with "end"

count      # calls the method
           # prints 1 2 3 4 5 6 7 8 9 10
```

Parameter and Argument
```
def square(n) # inbound variables are defined in the brackets
  puts n ** 2
end
square(12)
              # prints 144
```

Splat!
```
def what_up(greeting, *bros) # *var = splat:
                             # when you dont know how many following arguments
                             # there will be
  bros.each { |bro| puts "#{greeting}, #{bro}!" }
end

what_up("What up", "Justin", "Ben", "Kevin Sorbo")
                            # prints: What up, Justin!
                            #         What up, Ben!
                            #         What up, Kevin Sorbo!
```

Return
```
def cube(n)
  return n ** 3 # return "returns" the value out of the method
end
output = cube(3)
puts output
puts cube(2)
                # prints 27
                #        8
```

Blocks are like nameless methods
```
1.times { # blocks are defined by "{}" or "do end"
  puts "I'm a code block!"
}

1.times { puts "As am I!" }
          # prints: I'm a code block!
          #         As am I!
```

how blocks differ from methods
```
# method that capitalizes a word
def cap(string)
  puts "#{string.upcase}"
end
cap("ryan") # prints "RYAN"
cap("jane") # prints "JANE"

# block that capitalizes each string in the array
["ryan", "jane"].each {|string|
  puts "#{string}"
} # prints ryan
  #        jane
```

Intro to Sorting
```
my_array = [3, 4, 8, 7, 1, 6, 5, 9, 2]  # unsorted array
my_array.sort.each { |x|                # sort array, and then print each value
 print "#{x} "
}
print "\n"
                                        # prints 1 2 3 4 5 6 7 8 9
```

Foundations
```
books = ["Charlie and the Chocolate Factory",
         "War and Peace",
         "Utopia",
         "A Brief History of Time",
         "A Wrinkle in Time"]
books.sort.each {|x| puts x} # sort works on long strings too.
 # Prints: A Brief History of Time
 #         A Wrinkle in Time
 #         Charlie and the Chocolate Factory
 #         Utopia
 #         War and Peace
```

The combined Comparison Operator
```
x = 1
y = 2
z = x <=> y # "<=>" is the comparison operator
w = y <=> x
r = y <=> y
print "1 <=> 2 = #{z} \n" # is -1
print "2 <=> 1 = #{w} \n" # is 1
print "2 <=> 2 = #{r} \n" # is 0
```

Reverse Sorting
```
def az(arr, rev=false) # accepts 2 var's, "rev" default val = false
  if rev
    arr.sort { |item1, item2| item2 <=> item1 }
  else
    arr.sort { |item1, item2| item1 <=> item2 }
  end
end

array = [2,4,6,8,1,3,5,7,9]

# 1. spelled out method:
puts "1. spelled out method"
forward  = az(array)
backward = az(array, true)

puts "A-Z: #{forward}"            # prints A-Z: 123456789
puts "Z-A: #{backward}"           # prints Z-A: 987654321
puts "---"

# 2. simple method:
puts "2. simple method"
puts "A-Z: #{az(array)}"          # prints A-Z: 123456789
puts "Z-A: #{az(array, true)}"    # prints Z-A: 987654321
```





