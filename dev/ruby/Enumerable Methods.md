**Enumerables** are a set of convenient built-in methods in Ruby that are included as part of both arrays and hashes. Enumerables were designed to make implementing iteration patterns such as transforming, searching for, and selecting subsets of elements in collections much much easier.

## Life before enumerables

Let’s say that you wanted to make an invite list for your birthday using your `friends` array but that you don’t want to invite your friend Brian because he’s a bit of a nutcase at parties and always drinks way too much.
Using loops, you might do something like this:
```ruby
friends = ['sharon', 'Leo', 'Leila', 'Brian', 'Arun']
invited_list =[]

for friend in friends do
  if friend != 'Brian'
    invited_list.push(friend)
  end
end
#=> ["sharon", "Leo", "Leila", "Arun"]
```
That’s not too hard, but imagine having to do that for every party you host from now until the end of time! It might be easier to just stop hanging out with Brian 😁

Using the `#select` enumerable method, you could change the above code to this:
```ruby
invite_list = friends.select {|friend| friend != 'Brian'}
```
Or even using `#reject`
```ruby
invite_list = friends.reject {|friend| friend == 'Brian'}
```
This is pretty cool!

### The each method

`#each` is the granddaddy of the enumerable methods. It really can do anything, but it doesn't mean this method is the best or most efficient tool for the job.

Calling `#each` on an array will iterate through that array and will yield each element to a code block, where a task can be performed:
```ruby
friends.each {|friend| puts "hello, #{friend}"}
```

What if the block you want to pass to a method requires more logic than can fit on one line? It starts to become less readable and looks unwieldy. For multi-line blocks, the commonly accepted best practice is to change up the syntax to use `do..end` instead of `{...}`:
```ruby
my_nums = [1, 2, 3, 4]

my_nums.each do |num|
  num *= 2
  p "The new number is #{num}"
end
```

`#each` also works for hashes with a bit of added functionality. By default, each iteration will yield both the key and value individually or together (as an array) to the block depending on how you define your block variable:
```ruby
my_hash = {"one" => 1, "two" => 2}
my_hash.each {|key, val| puts "#{key} is #{val}"}
#=> one is 1
#=> two is 2

my_hash.each {|pair| puts "the pair is #{pair}"}
#=> the pair is ["one", 1]
#=> the pair is ["two", 2]
```

⚠️ The `#each` method returns the original array or hash regardless of what happens inside the code block. This is an important thing to keep in mind when debugging to avoid some confusion.
Take this as an example:
```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
puts friends.each { |friend| friend.upcase }
#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
```
You might expect this to return `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`, but you'd be wrong -- dead wrong. It actually return the original array you called `#each` on.

### The each_with_index method

This method is nearly the same as `#each`, but it provides some additional functionality by yielding two **block variables** instead of one as it iterates through an array. The first variable’s value is the element itself, while the second variable’s value is the index of that element within the array. This allows you to do things that are a bit more complex.

For example, If we only want to print every other word from an array of string:
```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]
fruits.each_with_index {|fruit, idx| p fruit if idx.even?}
#=> "apple"
#=> "strawberry"
```

⚠️ Just like with the `#each` method, `#each_with_index` returns the original array it’s called on.

### The map method

The `#map` method (also called `#collect`) transforms each element from an array according to whatever block you pass to it and returns the transformed elements in a new array. `#map` may seem confusing at first, but it is extremely useful.

If we go back using `#each`, we would need to create an extra variable to store transformed values:
```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]
upcased_fruits = []

fruits.each {|fruit| upcased_fruits.push(fruit.upcase)}

p upcased_fruits #=> ["APPLE", "BANANA", "STRAWBERRY", "PINEAPPLE"]
```
This is a bit cumbersome.
We can do same with `#map`:
```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]

upcased_fruits = fruits.map {|fruit| fruit.upcase}
p upcased_fruits
```

We can take this further with method like `#gsub`
```ruby
my_order = ['medium Big Mac', 'medium fries', 'medium milkshake']

p my_order.map {|item| item.gsub("medium", "extra large")}
#=> ["extra large Big Mac", "extra large fries", "extra large milkshake"]
```

### The select method

The `#select` method (also called `#filter`) passes every item in an array to a block and returns a new array with only the items for which the condition you set in the block evaluated to `true`.
```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

p friends.select { |friend| friend != 'Brian'}
#=> ["Sharon", "Leo", "Leila", "Arun"]
```

We can also use this method in a hash, just recall that when you use an enumerable method with a hash, you need to set up block variables for both the key and the value:
```ruby
responses = { 'Sharon' => 'yes', 'Leo' => 'no', 'Leila' => 'no', 'Arun' => 'yes' }

p responses.select {|person, answer| answer == 'yes'}
#=> {"Sharon"=>"yes", "Arun"=>"yes"}
```

### The reduce method

The `#reduce` method (also called `#inject`) is possibly the most difficult-to-grasp enumerable for new coders. The general idea is that it takes an array or hash and reduces it down to a single object. You should use `#reduce` when you want to get an output of a single value.

A classic example of when `#reduce` is useful is obtaining the sum of an array of numbers. First, let’s explore how we would achieve this using `#each`:
```ruby
my_numbers = [5, 6, 7, 8]
sum = 0

my_numbers.each {|num| sum += num}

p sum #=> 26
```

We can refactor this to use `reduce` method
```ruby
my_numbers = [5, 6, 7, 8]

p my_numbers.reduce {|sum, num| sum += num}
```
The first block variable in the `#reduce` enumerable (`sum` in this example) is known as the **accumulator**. The result of each iteration is stored in the accumulator and then passed to the next iteration. The accumulator is also the value that the `#reduce` method returns at the end of its work. By default, the initial value of the accumulator is the first element in the collection, so for each step of the iteration, we would have the following:
1. Iteration 0: sum = 5 + 6 = 11
2. Iteration 1: sum = 11 + 7 = 18
3. Iteration 2: sum = 18 + 8 = 26

We can also set a different value for the accumulator by directly passing in a value to the `#reduce` method.
```ruby
my_numbers = [5, 6, 7, 8]

p my_numbers.reduce(1000) {|sum, num| sum + num}
#=> 1026
```

We can also create with an empty hash:
```ruby
votes = ["Bob's Dirty Burger Shack", "St. Mark's Bistro", "Bob's Dirty Burger Shack"]

result = votes.reduce(Hash.new(0)) do |result, vote|
  result[vote] += 1
  result
end

p result #=> {"Bob's Dirty Burger Shack"=>2, "St. Mark's Bistro"=>1}
```
This `Hash.new` accepts an argument which sets the default value, and if no argument is passed in, it is defaulted to `nil`.
```ruby
hundreds = Hash.new(100)
hundreds["first"]         #=> 100
hundreds["mine"]          #=> 100
hundreds["yours"]         #=> 100
```
Once you set the value for a key equal to something else, the default value is overwritten:
```ruby
hundreds = Hash.new(100)
hundreds["new"]           #=> 100
hundreds["new"] = 99
hundreds["new"]           #=> 99
```

Now that we know that this new hash with a default value of `0` is our accumulator (which is called `result` in the code block), let’s see what happens in each iteration:

1. Iteration 0:
    - result = {}
    - Remember, this hash already has default values of `0`, so `result["Bob's Dirty Burger Shack"] == 0` and `result["St. Mark's Bistro"] == 0`
2. Iteration 1:
    - The method runs `result["Bob's Dirty Burger Shack"] += 1`
    - result = {“Bob’s Dirty Burger Shack” => 1}
3. Iteration 2:
    - The method runs `result["St. Mark's Bistro"] += 1`
    - result = {“Bob’s Dirty Burger Shack” => 1, “St. Mark’s Bistro” => 1}
4. Iteration 3:
    - The method runs `result["Bob's Dirty Burger Shack"] += 1`
    - result = {“Bob’s Dirty Burger Shack” => 2, “St. Mark’s Bistro” => 1}

### Bang methods

Earlier, we mentioned that enumerables like `#map` and `#select` return new arrays but don’t modify the arrays that they were called on. This is by design since we won’t often want to modify the original array or hash and we don’t want to accidentally lose that information. For example, if enumerables did mutate the original array, then using `#select` to filter out Brian from our invitation list would _permanently_ remove him from our friends list. Whoa! That’s a bit drastic. Brian may be a nutcase at parties, but he’s still our friend.

If you wanted to change your `friends` array instead, you could use the band method `#map!`
```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.map! {|friend| friend.upcase}

p friends #=> ["SHARON", "LEO", "LEILA", "BRIAN", "ARUN"]
```
Now when we call our original `friends` array again, it returns the changed values from the `#map!` method. Instead of returning a new array, `#map!` modified our original array.

All bang methods are **destructive** and modify the object they are called on. Many of the enumerable methods that return new versions of the array or hash they were called on have a bang method version available, such as `#map!` and `#select!`.

Keep in mind that it's best practice to avoid using these methods.

### Return values of enumerables

We can definitely store the result of an enumerable method into a local variable when we want to reuse the result.
```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

invited_friends = friends.select { |friend| friend != 'Brian' }

friends
#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

invited_friends
#=> ["Sharon", "Leo", "Leila", "Arun"]
```

Even a better option would be to wrap it in a method definition:
```ruby
def invited_friends(friends)
	friends.select {|friend| friend != "Brian"}
end

invited_friends(friends)
```

Of course there's more methods in enumerables!
[codementor](https://www.codementor.io/ruby-on-rails/tutorial/rubys-swiss-army-knife-the-enumerable-module)
