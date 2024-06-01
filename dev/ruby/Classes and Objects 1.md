As we mentioned earlier, we use classes to create objects. When defining a class, we typically focus on two things: _state_ and _behaviors_. State refers to the data associated to an individual object (which are tracked by instance variables). Behaviors are what objects are capable of doing.

## Initialize a New Object

We'll still use our `GoodDog` class from before, but we'll be removing functionality that existed in the previous chapter and starting fresh. Let's modify the class by adding an `initialize` method.
```ruby
class GoodDog
	def initialize
		puts "This object was initialized!"
	end
end

sparky = GoodDog.new #=> This object was initialized!
```

The `initialize` method gets called every time you create a new object. That's odd, don't we call the `new` method when we create an object? Yes, calling the `new` class method eventually leads us to the `initialize` instance method. We'll talk about the difference between class methods and instance methods later. In the above example, instantiating a new `GoodDog` object triggered the `initialize` method and resulted in the string being outputted. We refer to the `initialize` method as a _constructor_, because it is a special method that builds the object when a new object is instantiated. It gets triggered by the `new` class method.

## Instance Variables

Now that we know how to use constructors in Ruby, let's create a new object and instantiate it with some state, like a name.
```ruby
class GoodDog
	def initialize(name)
	@name = name
	end
end
```

You'll notice something new here. The `@name` variable looks different because it has the `@` symbol in front of it. This is called an **instance variable**. It is a variable that exists as long as the object instance exists and it is one of the ways we tie data to objects. It does not "die" after the initialize method is run. It "lives on", to be referenced, until the object instance is destroyed. In the above example, our `initialize` method is defined with one parameter: `name`. You can pass arguments into the `initialize` method through the `new` method. Let's create an object using the `GoodDog` class from above:
```ruby
sparky = GoodDog.new("Sparky")
```

### Composition and Aggregation

In OOP languages, **composition** and **aggregation** are design principles used to establish relationships between classes. Both composition and aggregation involve using instance variables to hold references to other objects, but they differ in terms of the lifecycle and ownership of the objects involved.

#### Composition

Composition is a relationship where an object -- often called the container -- contains one or more objects of other classes as part of its state. In composition, the contained -- composed -- objects are tied directly to the container. That is, the lifetimes of the container and composed objects are dependent on each other. Composed objects are created when the container is created and destroyed when the container is destroyed.

In Ruby, composition is typically implemented using instance variables that are initialized via the constructor of the class. Here’s an example:
```ruby
class Engine
  def start
    puts "Engine starting..."
  end
end

class Car
  def initialize
    @engine = Engine.new # Engine instance is created when Car is created
  end

  def start
    @engine.start
  end
end

my_car = Car.new
my_car.start

#=> Engine starting...
```

In this example, `Car` has an `Engine`, and `Car` instances contain an `Engine` object. When the `Car` is instantiated, the `Engine` is also instantiated. Likewise, when the `Car` object is destroyed, the composed `Engine` object is also destroyed.

We can say that a container has a **has-a relationship** to its composed objects. That is, the container "has a" composed object.

#### Aggregation

Aggregation is a form of association that is less tightly coupled than composition. Unlike composition, the the lifetime of the contained objects does not depend on the lifetime of the container. The container may have a reference to the objects, and it may coordinate their operations, but those objects typically exist independently of the container.

Here's an example:
```ruby
class Passenger
end

class Car
  def initialize(passengers)
    @passengers = passengers # Passengers are given to the Car at creation
  end
end

# Passengers can exist without Car
passengers = [Passenger.new, Passenger.new]
my_car = Car.new(passengers)
```

In this case, `Car` instances have a list of `Passenger` objects, but these `Passenger` objects can exist independently of the `Car` instance. They can be passed to the `Car` object when it's instantiated or any time before the `Car` instance is destroyed. However, the `Passenger` objects will continue to live on after the `Car` object is destroyed.

As with composed objects, we can say that a container has a **has-a relationship** to its aggregated objects. That is, the container "has an" aggregated object.

#### Relation to Instance Variables

The relationships between a container class instance and its composed and aggregated objects is implemented with instance variables. These instance variables hold references to other objects, enabling the container class to access and interact with the contained objects' methods and properties. The main difference lies in the ownership and the lifecycle of the objects referenced by these instance variables:

- Composition: The container owns the contained objects, and their lifecycles are tightly linked.
- Aggregation: The container does not own the contained objects; they can exist independently.

## Instance Methods

We can access the instance variables:
```ruby
class GoodDog
  def initialize(name)
    @name = name
  end

  def speak
    "#{@name} Arf!"
  end
end

sparky = GoodDog.new("Sparky")
p sparky.speak #=> "Sparky Arf!"

fido = GoodDog.new("Fido")
p fido.speak #=> "Fido Arf!"
```

## Accessor Methods

We if just directly access the instance variable like `sparky.name`, it would raise a `NoMethodError: undefined method 'name' for...` error.
Since we can't access an instance variable, we'd need to create a method to simply return the instance variables. 
```ruby
class GoodDog
...
	def get_name
		@name
	end
...
end

sparky = GoodDog.new("Sparky")
p sparky.get_name #=> "Sparky"
```

This is a *getter* method. But what if we wanted to change `sparky`'s name? This is when we reach for a *setter* method.
```ruby
class Gooddog
...
	def set_name=name
		@name = name
	end
...
end

sparky = GoodDog.new("Sparky")
sparky.set_name = "Bingo"
p sparky.get_name #=> "Bingo"
```

As you can see, we've successfully changed `sparky`'s name to the string "Spartacus". The first thing you should notice about the _setter_ method `set_name=` is that Ruby gives us a special syntax to use it. To use the `set_name=` method normally, we would expect to do this: `sparky.set_name=("Spartacus")`, where the entire "set_name=" is the method name, and the string "Spartacus" is the argument being passed in to the method. Ruby recognizes that this is a _setter_ method and allows us to use the more natural assignment syntax: `sparky.set_name = "Spartacus"`. When you see this code, just realize there's a method called `set_name=` working behind the scenes, and we're just seeing some Ruby _syntactical sugar_.

As a convention, Rubyists typically want to name those _getter_ and _setter_ methods using the same name as the instance variable they are exposing and setting. We'll make the change to our code as well:
```ruby
class GoodDog
  def initialize(name)
    @name = name
  end

  def name # This was renamed from "get_name"
    @name
  end

  def name=(n) # This was renamed from "set_name="
    @name = n
  end

  def speak
    "#{@name} says arf!"
  end
end
```

Keep in mind that setter methods always return the value that is passed in as an argument, regardless of what happens inside the method. If the setter tries to return something other than the argument's value, it just ignores that attempt.
```ruby
class Dog
...
  def name=(name) # This is a setter method
    @name = name
    "haha!"
  end
...
end

sparky = Dog.new("Sparky")
sparky.name = "Bingo"
p sparky.get_name #=> "Bingo"
```

You'll notice that writing those _getter_ and _setter_ methods took up a lot of room in our program for such a simple feature. And if we had other states we wanted to track, like height or weight, the class would be even longer. Because these methods are so commonplace, Ruby has a built-in way to automatically create these _getter_ and _setter_ methods for us, using the **attr_accessor** method. Check out this refactoring of the code from above.
```ruby
class GoodDog
  attr_accessor :name
  
  def initialize(name)
    @name = name
  end

  def speak
    "#{@name} Arf!"
  end
end

sparky = GoodDog.new("Sparky")
p sparky.speak #=> "Arf!"
p sparky.name #=> "Sparky"
sparky.name = "Bingo"
p sparky.name #=> "Bingo"
```

Our output is the same! The `attr_accessor` method takes a symbol as an argument, which it uses to create the method name for the `getter` and `setter` methods. That one line replaced two method definitions.

But what if we only want the `getter` method without the `setter` method? Then we would want to use the `attr_reader` method. It works the same way but only allows you to retrieve the instance variable. And if you only want the setter method, you can use the `attr_writer` method. All of the `attr_*` methods take `Symbol` objects as arguments; if there are more states you're tracking, you can use this syntax:
```ruby
attr_accessor :name, :height, :age
```

### Accessor Methods in Action

With getter and setter methods, we have a way to expose and change an object's state. We can also use these methods from within the class as well.
In the previous section, the `speack` method referenced the `@name` instance variable:
```ruby
def speak
	"#{@name} says arf!"
end
```

instead of referencing the instance variable directly, we want to use the name getter method that we created earlier, and that is given to use now by `attr_accessor`:
```ruby
def speak
	"#{name} says arf!"
end
```

We have removed the `@` symbol, and we're now calling the instance method, rather than the instance variable.

Why do this? Why not just reference the `@name` instance variable, like we did before? Technically, you could just reference the instance variable, but it's generally a good idea to call the _getter_ method instead.

Suppose we're keeping track of social security numbers in an instance variable called `@ssn`. And suppose that we don't want to expose the raw data, i.e. the entire social security number, in our application. Whenever we retrieve it, we want to only display the last 4 digits and mask the rest, like this: "xxx-xx-1234". If we were referencing the `@ssn` instance variable directly, we'd need to sprinkle our entire class with code like this:
```ruby
"xxx-xx" + @ssn.split("-").last
```

And what if we find a bug in this code, or if someone says we need to change the format to something else? It's much easier to just reference a getter method, and make the change in one place.

```ruby
def ssn
	# converts '123-45-6789' to 'xxx-xx-6789'
	"xxx-xx" + @ssn.split("-").last	
end
```

Now we can use the `ssn` instance method (note without the `@`) throughout our class to retrieve the social security number. Following this practice will save you some headache down the line. Just like the getter method, we also want to do the same with the setter method. Wherever we're changing the instance variable directly in our class, we should instead use the setter method. But there's a gotcha, which we'll cover next.

Suppose we added two more states to track to the `GoodDog` class called "height" and "weight":
```ruby
attr_accessor :name, :height, :weight
```

This one line of code gives us six getter/setter instance methods: `name`, `name=`, `height`, `height=`, `weight`, `weight=`. It also gives us three instance variables: `@name`, `@height`, `@weight`. Now suppose we want to create a new method that allows us to change several states at once, called `change_info(n, h, w)`. The three arguments to the method correspond to the new name, height, and weight, respectively. We could implement it like this:
```ruby
def change_info(n, h, w)
  @name = n
  @height = h
  @weight = w
end
```

Just to get caught up with all of our code, our entire `GoodDog` class now looks like the code below. Note the change to the `initialize` method and also the new method `change_info`. Finally, we created another method called `info` that displays all the states of the object, just for convenience:
```ruby
class GoodDog
  attr_accessor :name, :height, :weight

  def initialize(n, h, w)
    @name = n
    @height = h
    @weight = w
  end

  def speak
    "#{name} says arf!"
  end

  def change_info(n, h, w)
    @name = n
    @height = h
    @weight = w
  end

  def info
    "#{name} weighs #{weight} and is #{height} tall."
  end
end
```

And we can use the `change_info` method like this:
```ruby
sparky = GoodDog.new("Sparky", "12 inches", "10 lbs")
p sparky.info #=> "Sparky weights 10 lbs and is 12 inches tall."

sparky.change_info("Bingo", "24 inches", "45lbs")
p sparky.info #=> "Bingo weights 45lbs and is 24 inches tall."
```

Just like when we replaced accessing the instance variable directly with getter methods, we can do the same with our setter methods:
```ruby
...
  def change_info(n, h, w)
    name = n
    height = h
    weight = w
  end
...
```

But notice how `change_info` method didn't change `sparky`'s information:
```ruby
sparky.change_info("Bingo", "24 inches", "45lbs")
p sparky.info #=> "Sparky weights 10 lbs and is 12 inches tall."
```

WTH?
Relax!

### Calling Methods With self

The reason our setter methods didn't work is because Ruby thought we were initializing local variables. Recall that to initialize or create new local variables, all we have to do is `x = 1` or `str = "hello world"`. It turns out that instead of calling the `name=`, `height=` and `weight=` setter methods, what we did was create three new local variables called `name`, `height` and `weight`. That's definitely not what we wanted to do.

To disambiguate from creating a local variable, we need to use `self.name=` to let Ruby know that we're calling a method. So our `change_info` code should be updated to this:
```ruby
def change_info(n, h, w)
  self.name = n
  self.height = h
  self.weight = w
end
```

This tells Ruby that we're calling a setter method, not creating a local variable. To be consistent, we could also adopt this syntax for the getter methods as well, though it is not required.
```ruby
def info
  "#{self.name} weighs #{self.weight} and is #{self.height} tall."
end
```
(I personally think using `self` is much more informative).

Finally, if we run our code with the updated `change_info` method that uses the `self.` syntax, our code works beautifully:
```ruby
sparky.change_info('Spartacus', '24 inches', '45 lbs')
p sparky.info #=> "Bingo weights 45lbs and is 24 inches tall"
```

Note that prefixing `self.` is not restricted to just the accessor methods; you can use it with any instance method. For example, the `info` method is not a method given to us by `attr_accessor`, but we can still call it using `self.info`:
```ruby
class GoodDog
  # ... rest of code omitted for brevity
  def some_method
    self.info
  end
end
```

Here's some short, easy example of what we've learned so far:
```ruby
class MyCar
  attr_accessor :year, :color, :model, :current_speed
  
  def initialize(year, color, model)
    @year = year
    @color = color
    @model = model
    @current_speed = 0
  end

  def speed_up(speed)
    self.current_speed += speed
    puts "Accelerated to #{speed} mph."
  end

  def brake(speed)
    self.current_speed -= speed
    puts "Decelerated to #{speed} mph."
  end

  def check_speed
    puts "You are going at #{self.current_speed} mph."
  end

  def shut_down
    self.current_speed = 0
    puts "Let's park this bad boy!"
  end
end

fl5 = MyCar.new(2024, "white", "Civic Type R")
fl5.speed_up(60) #=> Accelerated to 60 mph.
fl5.brake(20) #=> Decelerated to 20 mph.
fl5.check_speed #=> You are going at 40 mph.
fl5.shut_down #=> Let's park this bad boy!
```
