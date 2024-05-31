Let's see how to use nested arrays and nested hashes to store more complex data.

## Nested Arrays

Nested array, or multimimensional array
```ruby
test_scores = [
  [97, 76, 79, 93],
  [79, 84, 76, 79],
  [88, 67, 64, 76],
  [94, 55, 67, 81]
]

teacher_mailboxes = [
  ["Adams", "Baker", "Clark", "Davis"],
  ["Jones", "Lewis", "Lopez", "Moore"],
  ["Perez", "Scott", "Smith", "Young"]
]
```

### Accessing elements

You can access nested array by index:
```ruby
p teacher_mailboxes[0][2] #=> "Clark"
p teacher_mailboxes[-1][-2] #=> "Smith"
```

If you try to access an index of a nonexistent nested element, it will raise an `NoMethoderror`, because the nil class does not have a `[]` method. However, just like a regular array, if you try to access a nonexistent index inside of an existing nested element, it returns a `nil`.
```ruby
teacher_mailboxes[3][0] #=> NoMethodError
teacher_mailboxes[0][4] #=> nil
```

If you want a `nil` value when trying to access an index of a nonexistent nested element, you can use the `#dig` method.
```ruby
teacher_mailboxes.dig(3, 0) #=> nil
teacher_mailboxes.dig(0, 4) #=> nil
```

### Creating a new nested array

We can create a new array by calling the Array.new method with up to 2 optional arguments (initial size and default value), like `Array.new(3)` or `Array.new(3, 7)`. However, there is one major “gotcha” that is important to point out. According to the [documentation](https://docs.ruby-lang.org/en/3.3/Array.html) the second optional argument, for the default value, should only be used with an immutable (unable to be changed) object such as a number, boolean value, or symbol. Using a string, array, hash, or other mutable object may result in confusing behavior because each default value in the array will actually be a _reference to_ the same default value. Therefore, any change to **one** of the elements will change **all** of the elements in the array.

To create an immutable array of mutable objects (string, array, hash, etc), you will need to pass the default value for `Array.new` via a block, using curly braces, instead of the second optional argument. The code in the block gets evaluated for every slot in the array, creating multiple objects to initialize the array with, rather than references to the same object.

Here's an example using the second optional argument for the default value:
```ruby
mutable = Array.new(3, Array.new(2))
p mutable #=> [[nil, nil], [nil, nil], [nil, nil]]

mutable[0][0] = 1000
p mutable #=> [[1000, nil], [1000, nil], [1000, nil]]
```
Changing the value of the first element in the first nested array, causes the first element to change in all three nested arrays! This same behavior will happen with strings, hashes, or any other mutable objects.

Let's take a look at an example that omits the second optional argument and instead passes in the mutable value in a block:
```ruby
mutable = Array.new(3) {Array.new(2)}
p mutable #=> [[nil, nil], [nil, nil], [nil, nil]]

mutable[0][0] = 'what?!'
p mutable #=> [["what?!", nil], [nil, nil], [nil, nil]]
```
Changing the value of the first element in the first nested array does not cause the value to change in any other nested array.

### Adding and removing elements

You can add another element to the end of nested array using the `#push` method or the shovel operator `<<`. If you want to add an element to a specific nested array, you will need to specify the index of the nested array.

```ruby
test_scores = [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81]]

test_scores << [100, 99, 98, 97]
#=> [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81], [100, 99, 98, 97]]

p test_scores[0].push(100)
#=> [97, 76, 79, 93, 100]
```

We can also remove elements with `#pop`
```ruby
test_scores[0].pop #=> 100
```

### Iterating over a nested array

We can use the `#each_with_index` method to iterate over a nested array.
```ruby
teacher_mailboxes = [["Adams", "Baker", "Clark", "Davis"], ["Jones", "Lewis", "Lopez", "Moore"], ["Perez", "Scott", "Smith", "Young"]]

teacher_mailboxes.each_with_index do |row, row_index|
  puts "Row:#{row_index} = #{row}"
end
#=> Row:0 = ["Adams", "Baker", "Clark", "Davis"]
#=> Row:1 = ["Jones", "Lewis", "Lopez", "Moore"]
#=> Row:2 = ["Perez", "Scott", "Smith", "Young"]
#=> [["Adams", "Baker", "Clark", "Davis"], ["Jones", "Lewis", "Lopez", "Moore"], ["Perez", "Scott", "Smith", "Young"]]
```

To iterate over elements inside of each row:
```ruby
teacher_mailboxes.each_with_index do |row, row_index|
  row.each_with_index do |teacher, column_index|
    puts "Row:#{row_index} Column:#{column_index} = #{teacher}"
  end
end
#=> Row:0 Column:0 = Adams
#=> Row:0 Column:1 = Baker
#=> Row:0 Column:2 = Clark
#=> Row:0 Column:3 = Davis
#=> Row:1 Column:0 = Jones
#=> Row:1 Column:1 = Lewis
#=> Row:1 Column:2 = Lopez
#=> Row:1 Column:3 = Moore
#=> Row:2 Column:0 = Perez
#=> Row:2 Column:1 = Scott
#=> Row:2 Column:2 = Smith
#=> Row:2 Column:3 = Young
#=> [["Adams", "Baker", "Clark", "Davis"], ["Jones", "Lewis", "Lopez", "Moore"], ["Perez", "Scott", "Smith", "Young"]]
```

What about a more complicated example of nesting two predicate enumberables together.
Let's determine if any student scored higher than 80 on everything.
```ruby
test_scores = [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81]]

test_scores.any? do |scores|
  scores.all? { |score| score > 80 }
end

#=> false
```
Because none of the nested arrays have scores that are all over 80.

What if we switch `#any` and  `#all`?
```ruby
test_scores.all? do |scores|
  scores.any? { |score| score > 80 }
end

#=> true
```

This results are different, because now it it determining if **all** of the nested arrays contain **any** number over 80.

### Nested hashes

Here's some nested hashes
```ruby
vehicles = {
  alice: {year: 2019, make: "Toyota", model: "Corolla"},
  blake: {year: 2020, make: "Volkswagen", model: "Beetle"},
  caleb: {year: 2020, make: "Honda", model: "Accord"}
}
```

### Accessing data

```ruby
vehicles[:alice][:year] #=> 2019
vehicles[:alice][:model] #=> "Corolla"
```

Similar to nested arrays, if you try to access a key in a nonexistent nested hash, it will raise an `NoMethodError`, therefore you may want to use the `#dig` method.
```ruby
vehicles[:zoe][:year] #=> NoMethodError
vehicles.dig(:zoe, :year) #=> nil
vehicles[:alice][:color] #=> nil
vehicles.dig(:alice, :color) #=> nil
```

### Adding and removing data

```ruby
vehicles[:dave] = {year: 2021, make: "Ford", model: "Mustang"}
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :blake=>{:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Mustang"}}
```

You can also add an element to one of the nested hashes.
```ruby
vehicles[:dave][:color] = "red"
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :blake=>{:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape", :color=>"red"}}
```

Deleting one of the nested hashes will be just like a regular hash.
```ruby
vehicles.delete(:blake)
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape", :color=>"red"}}
```

To delete one of the key/value pairs inside of a nested hash,
```ruby
vehicles[:dave].delete(:color)
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :blake=>{:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Mustang"}}

```

### With methods

If we want to know who owns vehicles that are from 2020 or newer. At first glance in the documentation, it looks like the `#select` would be a great method to use:
```ruby
puts vehicles.select {|name, data| data[:year] >= 2020}
#=> {:blake=>{:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}}
```

Cool, using `#select` gives us the information that we need. But, what if we only want the names of the owners and not another nested hash.
The `#collect` method sounds very useful for this situation.
```ruby
vehicles.collect {|name, data| name if data[:year] >= 2020}
#=> [nil, :blake, :caleb]
```

We are close! If you look at the documentation, you will see that `#collect` and `#map` have the same functionality. Both use the return value of each iteration, so when the if statement is false, it returns a `nil` value.

`nil` values can cause problems down the road, let's look through the documentation to find if there's a more helpful method to solve our problem.
The `#compact` method returns an array (or hash) without `nil` values.
```ruby
vehicles.collect {|name, data| name if data[:year] >= 2020}.compact
#=> [:blake, :caleb]
```

Nice! Using `#collect` and `#compact` returns the data that we want.
Ruby version 2.7 added a new enumerable method called `#filter_map` that sounds very useful for this situation.
```ruby
vehicles.filter_map {|name, data| name if data[:year] >= 2020}
#=> [:blake, :caleb]
```
🙀



