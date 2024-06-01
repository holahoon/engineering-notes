## Class Methods

Thus far, all the methods we've created are instance methods. That is, they are methods that pertain to an instance or object of the class. There are also class level methods, called **class methods**. Class methods are methods we can call directly on the class itself, without having to instantiate any objects

When defining a class method, we prepend the method name with the reserved word `self.`, like this:
```ruby
# ... rest of code ommitted for brevity
def self.what_am_i
	"I'm a GoodDog class!"
end
```

Then when we call the class method, we use the class name `GoodDog` followed by the method name, without having to instantiate any objects:
```ruby
GoodDog.what_am_i #=> I'm a GoodDog class!
```

## Class Variables

Just as instance variables capture information related to specific instances of classes (i.e., objects), we can create variables for an entire class that are appropriately named **class variables**. Class variables are created using two `@` symbols like so: `@@`. Let's create a class variable and a class method to view that variable.
```ruby
class GoodDog
  @@number_of_dogs = 0

  def initialize
    @@number_of_dogs += 1
  end

  def self.total_number_of_dogs
    @@number_of_dogs
  end
end

p GoodDog.total_number_of_dogs #=> 0

dog1 = GoodDog.new
dog2 = GoodDog.new

p GoodDog.total_number_of_dogs #=> 2
```

We have a class variable called `@@number_of_dogs`, which we initialize to 0. Then in our constructor (the `initialize` method), we increment that number by 1. Remember that `initialize` gets called every time we instantiate a new object via the `new` method. This also demonstrates that we can access class variables from within an instance method (`initialize` is an instance method). Finally, we just return the value of the class variable in the class method `self.total_number_of_dogs`. This is an example of using a class variable and a class method to keep track of a class level detail that pertains only to the class, and not to individual objects.

## Constants

When creating classes there may also be certain variables that you never want to change. You can do this by creating what are called **constants**. You define a constant by using an upper case letter at the beginning of the variable name. While technically constants just need to begin with a capital letter, most Rubyists will make the entire variable uppercase.

```ruby
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age = a * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
p sparky.age #=> 28
```

Here we used the constant `DOG_YEARS` to calculate the age in dog years when we created the object, `sparky`. Note that we used the setter methods in the `initialize` method to initialize the `@name` and `@age` instance variables given to us by the `attr_accessor` method. We then used the `age` getter method to retrieve the value from the object.

`DOG_YEARS` is a variable that will never change for any reason so we use a constant. It is possible to reassign a new value to constants but Ruby will throw a warning.

## The to_s Method

The `to_s` instance method comes built in to every class in Ruby. In fact, we have been using it all along. For example, suppose we have the `GoodDog` class from above, and the `sparky` object as well from above.

```ruby
puts sparky      # => #<GoodDog:0x007fe542323320>
```

What's happening here is that the `puts` method automatically calls `to_s` on its argument, which in this case is the `sparky` object. In other words `puts sparky` is equivalent to `puts sparky.to_s`. The reason we get this particular output lies within the `to_s` method in Ruby. By default, the `to_s` method returns the name of the object's class and an encoding of the object id.

> Note: `puts` method calls `to_s` for any argument that is not an array. For an array, it writes on separate lines the result of calling `to_s` on each element of the array.

To test this, we can add a custom `to_s` method to our `GoodDog` class, overriding the default `to_s` that comes with Ruby.

```ruby
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age = a * DOG_YEARS
  end

  def to_s
    "This dog's name is #{self.name} and it is #{self.age} in dog years."
  end
end

sparky = GoodDog.new("Sparky", 4)
puts sparky #=> This dog's name is Sparky and it is 28 in dog years.
```

There's another method called `p` that's very similar to `puts`, except it doesn't call `to_s` on its argument; it calls another built-in Ruby instance method called `inspect`. The `inspect` method is very helpful for debugging purposes, so we don't want to override it.
```ruby
p sparky #=> #<GoodDog:0x00007f50f2670210 @name="Sparky", @age=28>
```
This output implies that `p sparky` is equivalent to `puts sparky.inspect`.

Besides being called automatically when using `puts`, another important attribute of the `to_s` method is that it's also automatically called in string interpolation. We've seen this before when using integers or arrays in string interpolation:

```irb
irb :001 > arr = [1, 2, 3]
=> [1, 2, 3]
irb :002 > x = 5
=> 5
irb :003 > "The #{arr} array doesn't include #{x}."
=> The [1, 2, 3] array doesn't include 5.
```

Here, the `to_s` method is automatically called on the `arr` array object, as well as the `x` integer object. We'll see if we can include our `sparky` object in a string interpolation:

```irb
irb :001 > "#{sparky}"
=> "This dog's name is Sparky and it is 28 in dog years."
```

In summary, the `to_s` method is called automatically on the object when we use it with `puts` or when used with string interpolation. This fact may seem trivial at the moment, but knowing when `to_s` is called will help us understand how to read and write better OO code.

### Overriding #to_s

As shown above, you can customize the behavior of `#to_s` in a class. If you don't customize `#to_s`, Ruby looks up the inheritance chain for another version of `#to_s`, which is usually `Object#to_s`.

When overriding (customizing) `#to_s` for use in a custom class, you **must** remember that Ruby expects `#to_s` to always return a string. If it does not return a string, `#to_s` won't work as expected in places where `#to_s` is implicitly invoked like `puts` and interpolation. Instead of printing (or inserting) the value returned by `#to_s`, Ruby will ignore the non-string value and look in the inheritance chain for another version of `#to_s` that does return a string. In most cases, it will use the value returned by`Object#to_s` instead. For instance:
```ruby
class Foo
  def to_s
    42
  end
end

foo = Foo.new
puts foo             # Prints #<Foo:0x0000000100760ec0>
puts "foo is #{foo}" # Prints: foo is #<Foo:0x0000000100760ec0>
```

If you change `42` to a string, then the code will work as intended:
```ruby
class Foo
  def to_s
    "42"
  end
end

foo = Foo.new
puts foo             # Prints 42
puts "foo is #{foo}" # Prints: foo is 42
```

It's also worth noting that overridding `#to_s` only works for objects of the type where the customized `#to_s` method is defined. In particular, if you have a `Bar` object named `bar` that has an attribute named `xyz` and a `Bar#to_s` method, then `puts bar.xyz` **will not** use the customized `#to_s`. The value returned by `xyz` is not a `Bar` object, so `Bar#to_s` does not apply it:
```ruby
class Bar
  attr_reader :xyz
  def initialize
    @xyz = { a: 1, b: 2 }
  end

  def to_s
    'I am a Bar object!'
  end
end

bar = Bar.new
puts bar       # Prints I am a Bar object!
puts bar.xyz   # Prints {:a=>1, :b=>2}
```

## More About self

We talked about `self` earlier, but let's try to dive a little deeper so you can understand exactly what `self` is and how to understand what it's referencing. `self` can refer to different things depending on where it is used.

For example, so far we've seen two clear use cases for `self`:

1. Use `self` when calling setter methods from within the class. In our earlier example we showed that `self` was necessary in order for our `change_info` method to work properly. We had to use `self` to allow Ruby to disambiguate between initializing a local variable and calling a setter method.    
2. Use `self` for class method definitions.
    
Let's play around with `self` to see why the above two rules work. Let's assume the following code:
```ruby
class GoodDog
  attr_accessor :name, :height, :weight

  def initialize(n, h, w)
    self.name   = n
    self.height = h
    self.weight = w
  end

  def change_info(n, h, w)
    self.name   = n
    self.height = h
    self.weight = w
  end

  def info
    "#{self.name} weighs #{self.weight} and is #{self.height} tall."
  end
end
```

We use `self` whenever we call an instance method from within the class. We know the rule to use `self`, but what does `self` really represent here?
```ruby
class GoodDog
...
	def what_is_self
		self
	end
...

sparky = GoodDog.new('Sparky', '12 inches', '10 lbs')
p sparky.what_is_self
# => #<GoodDog:0x007f83ac062b38 @name="Sparky", @height="12 inches", @weight="10 lbs">
```

That's interesting. From within the class, when an instance method uses `self`, it references the _calling object_. In this case, that's the `sparky` object. Therefore, from within the `change_info` method, calling `self.name=` acts the same as calling `sparky.name=` from _outside_ the class (you can't call `sparky.name=` inside the class, though, since it isn't in scope). Now we understand why using `self` to call instance methods from within the class works the way it does!

The other place we use `self` is when we're defining class methods, like this:
```ruby
class MyAwesomeClass
	def self.this_is_a_class_method
	end
end
```

When `self` is prepended to a method definition, it is defining a **class method**. We talked about these earlier. In our `GoodDog` class method example, we defined a class method called `self.total_number_of_dogs`. This method returned the value of the class variable `@@number_of_dogs`. How is this possible? Let's add a line to our `GoodDog` class:
```ruby
class GoodDog
  # ... rest of code omitted for brevity
  puts self
end
```

If you run the `good_dog.rb` file with the `GoodDog` class definition, you'll see that `GoodDog` is output. Thus, you can see that using `self` inside a class but outside an instance method refers to the class itself. Therefore, a method definition prefixed with `self` is the same as defining the method on the class. That is, `def self.a_method` is equivalent to `def GoodDog.a_method`. That's why it's a class method; it's actually being defined on the class. Using `self.a_method`, rather than `ClassName.a_method` is a convention. It is useful because if in the future we rename the class, we only have to change the name of the class, rather than having to rename all of the class methods too.

To be clear, from within a class...

1. `self`, inside of an instance method, references the instance (object) that called the method - the calling object. Therefore, `self.weight=` is the same as `sparky.weight=`, in our example.
2. `self`, outside of an instance method, references the class and can be used to define class methods. Therefore if we were to define a `name` class method, `def self.name=(n)` is the same as `def GoodDog.name=(n)`

Thus, we can see that `self` is a way of being explicit about what our program is referencing and what our intentions are as far as behavior. `self` changes depending on the scope it is used in, so pay attention to see if you're inside an instance method or not.
