## Creating arrays
Arrays are commonly created with an **array literal**, which is a special syntax used to create instances of an array object. This is done using `[]`.
```ruby
num_array = [1, 2, 3, 4]
str_array = ["this", "is", "a", "small", "array"]
```

An array can also be created by calling the `Array.new` method which takes up to 2 optional arguments (initial size and default value):
```ruby
Array.new #=> []
Array.new(3) #=> [nil, nil, nil]
Array.new(3, 7) #=> [7, 7, 7]
```

## Accessing arrays
We can access the elements in array like:
```ruby
str_array[0] #=> "this"
str_array[-1] #=> "array"
```

Ruby provides the `#first` and `#last` array methods. These methods take an integer argument which will return a new array that contains the first or last `n` elements of the array.
```ruby
str_array.first #=> "this"
str_array.first(3) #=> ["this", "is", "a"]
str_array.last(2) #=> ["small", "array"]
```

## Adding and Removing elements
Adding an element to an existing array is done by using the `#push` method or the shovel operator `<<`. Both methods will ad elements to the end of an array and return that array with the new elements.
The `#pop` method will remove the element at the end of an array and return the element that was removed.
```ruby
num_array = [1, 2]

num_array.push(3, 4)      #=> [1, 2, 3, 4]
num_array << 5            #=> [1, 2, 3, 4, 5]
num_array.pop             #=> 5
num_array                 #=> [1, 2, 3, 4]
```

The methods `#shift` and `#unshift` are used to add and remove elements at the beginning of an array. The `#unshift` method adds elements to the beginning of an array and returns that array (much like `#push`). The `#shift` method removes the first element of an array and returns that element (much like `#pop`).
```ruby
num_array = [2, 3, 4]

num_array.unshift(1)      #=> [1, 2, 3, 4]
num_array.shift           #=> 1
num_array                 #=> [2, 3, 4]
```

It’s also useful to know that both `#pop` and `#shift` can take integer arguments:
```ruby
num_array = [1, 2, 3, 4, 5, 6]

num_array.pop(3)          #=> [4, 5, 6]
num_array.shift(2)        #=> [1, 2]
num_array                 #=> [3]
```

## Adding and subtracting arrays
We can use the `+` or `concat` method to add arrays.
```ruby
a = [1, 2, 3]
b = [3, 4, 5]

a + b         #=> [1, 2, 3, 3, 4, 5]
a.concat(b)   #=> [1, 2, 3, 3, 4, 5]
```

To find the difference between two arrays, you can subtract them using `-`. This method returns a copy of the first array, removing any elements that appear in the second array.
```ruby
[1, 1, 1, 2, 2, 3, 4] - [1, 4]  #=> [2, 2, 3]
```

## Basic methods

Ruby provides many methods to manipulate arrays and their contents (over 150!) Here's the [Array docs](https://docs.ruby-lang.org/en/3.3/Array.html).
We can even see a list of the available methods:
```ruby
num_array.methods
```

Here are some examples:
```ruby
[].empty?               #=> true
[[]].empty?             #=> false
[1, 2].empty?           #=> false

[1, 2, 3].length        #=> 3

[1, 2, 3].reverse       #=> [3, 2, 1]

[1, 2, 3].include?(3)   #=> true
[1, 2, 3].include?("3") #=> false

[1, 2, 3].join          #=> "123"
[1, 2, 3].join("-")     #=> "1-2-3"
```
