# Arrays and Hashes

Declare an Array
```
my_array = [1,2,3]
string_array ["bob","joe","marry"]
```

Access Array values by index
```
demo_array = [100, 200, 300, 400, 500]
print  demo_array[2]       # prints "300"
```

Arrays of Arrays (.multidimensional arrays)
```
multi_d_array = [[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,0]]
multi_d_array.each { |x| puts "#{x}\n" }  # prints 0000
                                          #        0000
                                          #        0000
                                          #        0000
```

Intro to Hashes
```
my_hash = { "name" => "Eric",
  "age" => 26,
  "hungry?" => true
}
puts my_hash["name"]     # prints Eric
puts my_hash["age"]      # prints 26
puts my_hash["hungry?"]  # prints true
```

using Hash.new
```
pets = Hash.new  # creates a new hash called pets
```

adding to a hash/ accessing a hash value
```
pets = Hash.new
pets["willow"] = "dog"  # add willow => dog to hash
puts pets["willow"]     # prints dog
```

Iterating over Arrays and Hashes
```
friends = ["Milhouse", "Ralph", "Nelson", "Otto"]

family = { "Homer" => "dad",
  "Marge" => "mom",
  "Lisa" => "sister",
  "Maggie" => "sister",
  "Abe" => "grandpa",
  "Santa's Little Helper" => "dog"
}

friends.each { |x| puts "#{x}" }
puts "---"
family.each { |x, y| puts "#{x}: #{y}" }

# prints:
# Milhouse
# Ralph
# Nelson
# Otto
# ---
# Abe: grandpa
# Marge: mom 
# Santa's Little Helper: dog  
# Maggie: sister
# Homer: dad
# Lisa: sister
```

Iterating over arrays
```
languages = ["HTML", "CSS", "JavaScript", "Python", "Ruby"]
languages.each { |x|
 print " #{x} " # prints  HTML  CSS  JavaScript  Python  Ruby
}
print "\n"
```

Iterating over multidimensional Arrays
```
s = [["ham", "swiss"], ["turkey", "cheddar"], ["roast beef", "gruyere"]]
s.each { |s2|   # go though the first array
 s2.each { |x|  # then view the contents of the second and print
  print "#{x} "
 }
 print "\n"
}
```

Iterating over hashes
```
secret_identities = { "The Batman" => "Bruce Wayne",
  "Superman" => "Clark Kent",
  "Wonder Woman" => "Diana Prince",
  "Freakazoid" => "Dexter Douglas"
}
secret_identities.each { |key, value|
 puts "hero = #{key}, person = #{value}"
}
    # prints: hero = Freakazoid, person = Dexter Douglas
    #         hero = Superman, person = Clark Kent
    #         hero = Wonder Woman, person = Diana Prince
    #         hero = The Batman, person = Bruce Wayne
```


