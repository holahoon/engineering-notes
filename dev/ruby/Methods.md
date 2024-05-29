In Ruby, parentheses are *generally* optional. We could write `puts` in two different forms:
```ruby
puts "anything"
puts("anything")
```

## Creating a method

You can create your own custom methods:
```ruby
def my_name
	"DK"
end

puts my_name #=> "DK"
```
- `def` is a built-in keyword that tells Ruby that this is the start of a method definition.
- `my_name` is the name of your new method. You can name your methods almost anything you want, but there are some constraints and conventions.
- `"Joe Smith"` is the code inside the method body. All of the logic for your method is contained in the indented lines of the method body. This particular method returns a string when the method is called.
- `end` is a built-in keyword that tells Ruby that this is the end of the method definition.
- To call the method, you need to use its name, as shown in the last line of the example.

### Method names

As mentioned above, you can name your methods almost anything you want, but you shouldn’t pick names haphazardly. There are certain conventions that are recommended in order to improve the readability and maintainability of your code.

Your method names can use numbers, capital letters, lowercase letters, and the special characters `_`, `?`, `!`, and `=`. Just like with variable names in Ruby, the convention for a method name with multiple words is to use **snake_case**, separating words with underscores.

Here are some things you are not allowed to do with your method names:
- You cannot name your method one of Ruby’s approximately 40 reserved words, such as `end`, `while`, or `for`. Checkout the full list [here](http://www.java2s.com/Code/Ruby/Language-Basics/Rubysreservedwords.htm).
- You cannot use any symbols other than `_`, `?`, `!`, and `=`.
- You cannot use `?`, `!`, or `=` anywhere other than at the end of the name.
- You cannot begin a method name with a number.
```ruby
method_name      # valid
_name_of_method  # valid
1_method_name    # invalid
method_27        # valid
method?_name     # invalid
method_name!     # valid
begin            # invalid (Ruby reserved word)
begin_count      # valid
```

### Parameters and arguments

Of course, not all methods are as basic as the `my_name` example method above. After all, what good are methods if you can’t interact with them? When you want to return something other than a fixed result, you need to give your methods parameters. **Parameters** are variables that your method will receive when it is called. You can have more meaningful and useful interactions with your methods by using parameters to make them more versatile.
```ruby
def greet(name)
	"Hello, #{name}!"
end

puts greet("DK") #=> "Hello, DK!"
```

#### Default parameters
```ruby
def greet(name = "DK")
  "Hello, #{name}!"
end

puts greet #=> "Hello, DK!"
```

### Explicit and Implicit return

Generally, we define a method to return something. This is known as an **explicit return** statement:
```ruby
def my_name
	return "DK"
end
```
This method will return `"DK"`.
Ruby is one of the few languages that offers **implicit return** for methods, which means that Ruby method will return the last expression that was evaluated even without the `return` statement. The last expression that was evaluated may or may not be the last line in the code.
```ruby
def my_name
  "DK"
end
puts my_name #=> "DK"

def some_method(number)
  number = 7
end
puts some_method #=> 7

def even_odd(num)
  if num % 2 == 0
    "Even"
  else
    "Odd"
  end
end

puts even_odd(5) #=> "Odd"
```

Even though Ruby offers the ease of using implicit returns, explicit returns still have a place in Ruby code.
```ruby
def my_name
	return "DK"
	"David"
end

puts my_name #=> "DK"
```

For example, an explicit return can be useful when you want to write a method that checks for input errors before continuing.
```ruby
def even_odd(number)
  unless number.is_a? Numeric
    return "A number was not entered."
  end

  if number.even?
    "That is an even number."
  else
    "That is an odd number."
  end
end

puts even_odd(2) #=> That is an even number
puts even_odd("hi") #=> A number was not entered.
```

### Predicate methods
Methods with question mark `?` at the end of their name are **predicate** methods, which is a naming convention that Ruby uses that return a Boolean.
```ruby
puts 5.even? #=> false
puts 12.between?(10, 15) #=> true
```

### Band methods
```ruby
whipser = "HELLO EVERYBODY"

puts whipser.downcase #=> "hello everybody"
puts whipser #=> "HELLO EVERYBODY"
```
Just by calling a method on an object will not modify the original value of that object. Unless we reassign a variable:
```ruby
whipser = whipser.downcase
```

By adding a `!` to the end of your method, you indicate that this method performs its action and simultaneously overwrites the value of the original object with the result:
```ruby
puts whipser.downcase! #=> "hello everybody"
puts whiper #=> "hello everybody"
```
Writing `whisper.downcase!` is the equivalent of writing `whisper = whisper.downcase`.



