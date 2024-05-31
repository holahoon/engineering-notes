Predicate method is indicated by a question mark (`?`) at the end of the method name and returns either `true` or `false`.

### The include? method

The `#include?` method returns `true` if an argument exists in the array or hash; otherwise, returns `false`.
We can use `#each` function to achieve this:
```ruby
numbers = [5, 6, 7, 8]
element = 6
result = false

numbers.each do |num|
  if num == element
    result = true
    break
  end
end

p result #=> true
```
We just break out of the loop once the condition is met to avoid unnecessary loops.

Using `#include?`, this can be simplified:
```ruby
numbers.include?(6) #=> true
```
Example:
```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
invited_list = friends.select {|friend| friend != 'Brian'}
p invited_list.include?('Brian') #=> false
```

### The any? method

The `#any?` method returns `true` if *any* elements in an array or hash match the condition within the block; otherwise, `false`.

Let's how we could achieve this using `#each`:
```ruby
numbers = [21, 42, 303, 499, 550, 811]
result = false

numbers.each do |num|
  if num > 500
    result = true
    break
  end
end

p result #=> true
```

Using `#any?`:
```ruby
numbers.any? {|num| num > 500} #=> true
```

### The all? method

The `#all?` method is also fairly intuitive. It only return `true` if *all* the elements in the array or hash match the condition within the block; otherwise, `false`.

Let’s say that we want to check whether all the words in our list are more than 3 characters or 6 characters long. First, let’s see how we could achieve this using `#each`:
```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]
matches = []
result = false

fruits.each do |fruit|
  if fruit.length > 3
    matches.push(fruit)
  end
end

result = matches.length == fruits.length
p result #=> true
```

Using `#all?` method:
```ruby
fruits.all? {|fruit| fruit.length > 3} #=> true
fruits.all? {|fruit| fruit.length > 6} #=> false
```

### The none? method

The `#none?` returns `true` only if the condition matches *none* of the elements in the array or hash; otherwise, returns `false`.

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]
result = false

fruits.each do |fruit|
  if fruit.length > 10
    result = false
    break
  end

  result = true
end

p result #=> true
```

We can simplify with `#none?`:
```ruby
fruits.none? {|fruit| fruit.length > 10} #=> true
```
