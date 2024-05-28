## Creating hashes

```ruby
my_hash = {
  "a random word" => "ahoy",
  "Dorothy's math test score" => 94,
  "an array" => [1, 2, 3],
  "an empty hash within a hash" => {}
}
```
Hash consists of key, hash rocket(`=>`), and value.

Or you can create a hash the good old `::new` method on the `Hash` class.
```ruby
my_hash = Hash.new
```

Ruby is pretty flexible when it comes to keys and values in hashes.
```ruby
hash = {
	9 => "nine",
	:six => 6
}
```

## Accessing hashes
```ruby
shoes = {
	"summer" => "sandals",
	"winter" => "boots"
}

shows["summer"] #=> "sandals"
```

When accessing a key that doesn't exist in the hash, it will return `nil`
```ruby
shows["spring"] #=> nil
```

Sometimes, this behavior can be problematic for you if silently return `nil` value could potentially cause chaos in your program. Luckily, hashes have a `fetch` method that will raise an error when you try to access a key that is not in your hash.
```ruby
shoes.fetch("hiking") #=> KeyError: key not found: "hiking"
```
Alternatively, this method can return a default value instead of raising an error if the given key is not found:
```ruby
shoes.fetch("hiking", "hiking boots") #=> "hiking boots"
```

## Adding, removing and changing data

#### Adding
```ruby
shoes["fall"] = "sneakers"
#=> {"summer"=>"sandals", "winter"=>"boots", "fall"=>"sneakers"}
```

#### Changing
```ruby
shoes["summer"] = "flip-flops"
#=> {"summer"=>"flip flops", "winter"=>"boots", "fall"=>"sneakers"}
```

#### Removing
```ruby
shoes.delete("summer")
```

## Methods

Hashes respond to many of the same methods as arrays do since they both employ Ruby's **Enumerable** module.
A couple of useful methods that are specific to hashes are the `#keys` and `#values` method. Note that both of these methods return *arrays*.
```ruby
books = {
  "Infinite Jest" => "David Foster Wallace",
  "Into the Wild" => "Jon Krakauer"
}

puts books.keys #=> ["Infinite Jest", "Into the Wild"]
puts books.values #=> ["David Foster Wallace", "Jon Krakauer"]
```

## Merge two hashes
```ruby
hash1 = { "a" => 100, "b" => 200 }
hash2 = { "b" => 254, "c" => 300 }
hash1.merge(hash2) #=> { "a" => 100, "b" => 254, "c" => 300 }
```
Notice that the values from the hash getting merged in(`hash2`) overwrites the `hash1` when the two hashes have a key that's the same.

## Symbols as hash keys
In the real world, you'll almost always see symbols (like `:this_guy`) used as keys. This is predominantly because symbols are far more performant than strings in Ruby, but they also allow for a much cleaner syntax when defining hashes.
```ruby
# 'Rocket' syntax
american_cars = {
  :chevrolet => "Corvette",
  :ford => "Mustang",
  :dodge => "Ram"
}

# 'Symbols' syntax
japanese_cars = {
  honda: "Accord",
  toyota: "Corolla",
  nissan: "Altima"
}
```

That last example brings a tear to the eye, doesn’t it? Notice that the hash rocket and the colon that represents a symbol have been mashed together. This unfortunately only works for symbols, though, so don’t try `{ 9: "value" }` or you’ll get a syntax error.

When you use the cleaner ‘symbols’ syntax to create a hash, you’ll still need to use the standard symbol syntax when you’re trying to access a value. In other words, regardless of which of the above two syntax options you use when creating a hash, they both create symbol keys that are accessed the same way.

```ruby
american_cars[:ford] #=> "Mustang"
japanese_cars[:honda] #=> "Accord"
```

## Iterating over hashes
```ruby
person = {name: 'bob', height: '6 ft', weight: '160 lbs', hair: 'brown'}

person.each do |key, value|
  puts "Bob's #{key} is #{value}"
end

#=> Bob's name is bob
#=> Bob's height is 6 ft
#=> Bob's weight is 160 lbs
#=> Bob's hair is brown
```



