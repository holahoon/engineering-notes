**Inheritance** is when a class **inherits** behavior from another class. The class that is inheriting behavior is called the subclass and the class it inherits from is called the superclass.

We use inheritance as a way to extract common behaviors from classes that share that behavior, and move it to a superclass. This lets us keep logic in one place.

## Class Inheritance

Here, we're extracting the `speak` method from the `GoodDog` class to the superclass `Animal`, and we use inheritance to make that behavior available to `GoodDog` and `Cat` classes.

```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
end

class Cat < Animal
end

sparky = GoodDog.new
paws = Cat.new

puts sparky.speak #=> Hello!
puts paws.speak #=> Hello!
```

We use the `<` symbol to signify that the `GoodDog` class is inheriting from the `Animal` class. This means that all of the methods in the `Animal` class are available to the `GoodDog` class for use. We also created a new class called `Cat` that inherits from `Animal` as well. We've eliminated the `speak` method from the `GoodDog` class in order to use the `speak` method from `Animal`.

But what if we want to use the original `speak` method from the `GoodDog` class only. Let's add it back and see what happens.
```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
  attr_accessor :name

  def initialize(n)
    self.name = n
  end

  def speak
    "#{self.name} says arf!"
  end
end

class Cat < Animal
end

sparky = GoodDog.new("Sparky")
paws = Cat.new

puts sparky.speak #=> Sparky says arf!
puts paws.speak #=> Hello!
```

In the `GoodDog` class, we're **overriding** the `speak` method in the `Animal` class because Ruby checks the object's class first for the method before it looks in the superclass. That means when we wrote the code `sparky.speak`, it first looked at `sparky`'s class, which is `GoodDog`. It found the `speak` method there and used it. When we wrote the code `paws.speak`, Ruby first looked at `paws`'s class, which is `Cat`. It did not find a `speak` method there, so it continued to look in `Cat`'s superclass, `Animal`. It found a `speak` method in `Animal`, and used it.

## super

Ruby provides us with the `super` keyword to call methods earlier in the method lookup path. When you call `super` from within a method, it searches the method lookup path for a method with the same name, then invokes it. Let's see a quick example of how this works:
```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
  def speak
    super + " from GoodDog class"
  end
end

sparky = GoodDog.new
sparky.speak # => "Hello! from GoodDog class"
```

In the above example, we've created a simple `Animal` class with a `speak` instance method. We then created `GoodDog` which subclasses `Animal` also with a `speak` instance method to override the inherited version. However, in the subclass' `speak` method we use `super` to invoke the `speak` method from the superclass, `Animal`, and then we extend the functionality by appending some text to the return value.

Another more common way of using `super` is with `initialize`. Let's see an illustration of that:
```ruby
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

class GoodDog < Animal
  def initialize(color)
    super
    @color = color
  end
end

p bruno = GoodDog.new("brown")
#=> #<GoodDog:0x00007b4723170760 @name="brown", @color="brown">
```

The interesting concept we want to explain is the use of `super` in the `GoodDog` class. In this example, we're using `super` with no arguments. However, the `initialize` method, where `super` is being used, takes an argument and adds a new twist to how `super` is invoked. Here, in addition to the default behavior, `super` automatically forwards the arguments that were passed to the method from which `super` is called (`initialize` method in `GoodDog` class). At this point, `super` will pass the `color` argument in the `initialize` defined in the subclass to that of the `Animal` superclass and invoke it. That explains the presence of `@name="brown"` when the `bruno` instance is created. Finally, the subclass' `initialize` continues to set the `@color` instance variable.

When called with specific arguments, eg. `super(a, b)`, the specified arguments will be sent up the method lookup chain. Let's see a quick example:
```ruby
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

class BadDog < Animal
  def initialize(age, name)
    super(name)
    @age = age
  end
end

p bruno = BadDog.new(10, "Max")
#=> #<BadDog:0x00007a60b1521728 @name="Max", @age=10>
```

This is similar to our previous example, with the difference being that `super` takes an argument, hence the passed in argument is sent to the superclass. Consequently, in this example when a `BadDog` object is created, the passed in `name` argument ("bear") is passed to the superclass and set to the `@name` instance variable.

There's one last twist. If you call `super()` exactly as shown -- with parentheses -- it calls the method in the superclass with no arguments at all. If you have a method in your superclass that takes no arguments, this is the safest -- and sometimes the only -- way to call it:
```ruby
class Animal
  def initialize
  end
end

class Bear < Animal
  def initialize(color)
    super()
    @color = color
  end
end

bear = Bear.new("black")        # => #<Bear:0x007fb40b1e6718 @color="black">
```

If you forget to use the parentheses here, Ruby will raise an `ArgumentError` exception since the number of arguments is incorrect.

## Mixing in Modules

Another way to DRY up your code in Ruby is to use **modules**.

Extracting common methods to a superclass, like we did in the previous section, is a great way to model concepts that are naturally hierarchical. We gave the example of animals. We have a generic superclass called `Animal` that can keep all basic behavior of all animals. We can then expand on the model a little and have, perhaps, a `Mammal` subclass of `Animal`. We can imagine the entire class hierarchy to look something like the figure below.

![[Pasted image 20240601203919.png]]
The above diagram shows what pure class based inheritance looks like. Remember the goal of this is to put the right behavior (i.e., methods) in the right class so we don't need to repeat code in multiple classes. We can imagine that all `Fish` objects are related to animals that live in the water, so perhaps a `swim` method should be in the `Fish` class. We can also imagine that all `Mammal` objects will have warm blood, so we can create a method called `warm_blooded?` in the `Mammal` class and have it return `true`. Therefore, the `Cat` and `Dog` objects will have access to the `warm_blooded?` method which is automatically inherited from `Mammal` by the `Cat` and `Dog` classes, but they won't have access to the methods in the `Fish` class.

This type of hierarchical modeling works, to some extent, but there are always exceptions. For example, we put the `swim` method in the `Fish` class, but some mammals can swim as well. We don't want to move the `swim` method into `Animal` because not all animals swim, and we don't want to create another `swim` method in `Dog` because that violates the DRY principle. For concerns such as these, we'd like to group them into a module and then _mix in_ that module to the classes that require those behaviors. Here's an example:
```ruby
module Swimmable
  def swim
    "I'm swimming!"
  end
end

class Animal; end

class Fish < Animal
  include Swimmable # Mixing in Swimmable module
end

class Mammal < Animal
end

class Cat < Mammal
end

class Dog < Mammal
  include Swimmable # Mixing in Swimmable module
end
```

And now `Fish` and `Dog` objects can swim, but objects of other classes won't be able to:
```ruby
sparky = Dog.new
neemo = Fish.new
paws = Cat.new

puts sparky.swim #=> I'm swimming!
puts neemo.swim #=> I'm swimming!
puts paws.swim #=> undefined method `swim' for #<Cat:0x00007b040385d400> (NoMethodError)
```

Using modules to group common behaviors allows us to build a more powerful, flexible and DRY design.

_Note: A common naming convention for Ruby is to use the "able" suffix on whatever verb describes the behavior that the module is modeling. You can see this convention with our `Swimmable` module. Likewise, we could name a module that describes "walking" as `Walkable`. Not all modules are named in this manner, however, it is quite common._

## Inheritance vs Modules

Now you know the two primary ways that Ruby implements inheritance. Class inheritance is the traditional way to think about inheritance: one type inherits the behaviors of another type. The result is a new type that specializes the type of the superclass. The other form is sometimes called **interface inheritance**: this is where mixin modules come into play. The class doesn't inherit from another type, but instead inherits the interface provided by the mixin module. In this case, the result type is not a specialized type with respect to the module.

You may wonder when to use class inheritance vs mixins. Here are a couple of things to consider when evaluating these choices.

- You can only subclass (class inheritance) from one class. You can mix in as many modules (interface inheritance) as you'd like.
- If an instance of class `B` can be described as a kind of class `A`, then we say that `B` and `A` have an **is-a relationship**. if such a relationship exists, then we probably want to use class inheritance such that class `B` inherits from class `A`.
- If class `B` and `A` do not have an is-a relationship, there may be a **has-a relationship** involved. We saw has-a relationships in conjunction with composition and aggregation, but such relationships also exist when interface existence is desired. If you can say that class `A` has the behaviors of type `B`, but `B` and `A` don't have an is-a relationship, then you probably want to define `B` as a module and use interface inheritance.
- Note that you cannot instantiate modules. In other words, objects cannot be created from modules.

## Method Lookup Path

Recall the method lookup path is the order in which classes are inspected when you call a method.
```ruby
module Walkable
  def walk
    "I'm walking"
  end
end

module Swimmable
  def swim
    "I'm swimming"
  end
end

module Climable
  def climb
    "I'm climbing"
  end
end

class Animal
  include Walkable

  def speak
    "I'm an animal, and I speak!"
  end
end

puts Animal.ancestors

# Animal
# Walkable
# Object
# Kernel
# BasicObject
```

We have three modules and one class. We've mixed in one module into the `Animal` class. The method lookup path is the path Ruby takes to look for a method. We can see this path with the `ancestors` class method.
This means that when we call a method of any `Animal` object, first Ruby looks in the `Animal` class, then the `Walkable` module, then the `Object` class, then the `Kernel` module, and finally the `BasicObject` class.

```ruby
fido = Animal.new
fido.speak                  # => I'm an animal, and I speak!
```
Ruby found the `speak` method in the `Animal` class and looked no further.

```ruby
fido.walk                   # => I'm walking.
```
Ruby first looked for the `walk` instance method in `Animal`, and not finding it there, kept looking in the next place according to our list, which is the `Walkable` module. It saw a `walk` method there, executed it, and stopped looking further.

```ruby
fido.swim
  # => NoMethodError: undefined method `swim' for #<Animal:0x007f92832625b0>
```
Ruby traversed all the classes and modules in the list, and didn't find a `swim` method, so it threw an error.

Let's add another class to the code above. This class will inherit from the `Animal` class and mix in the `Swimmable` and `Climable` modules:
```ruby
class GoodDog < Animal
  include Swimmable
  include Climable
end

puts GoodDog.ancestors

# GoodDog
# Climable
# Swimmable
# Animal
# Walkable
# Object
# Kernel
# BasicObject
```

There are several interesting things about the above output. First, this tells us that the order in which we include modules is important. Ruby actually looks at the last module we included _first_. This means that in the rare occurrence that the modules we mix in contain a method with the same name, the last module included will be consulted first. The second interesting thing is that the module included in the superclass made it on to the method lookup path. That means that all `GoodDog` objects will have access to not only `Animal` methods, but also methods defined in the `Walkable` module, as well as all other modules mixed in to any of its superclasses.

Sometimes when you're working on a large project, it can be confusing where all these methods are coming from. By understanding the method lookup path, we can have a better idea of where and how all available methods are organized.

## More Modules

The first use case we'll discuss is using modules for **namespacing**. In this context, namespacing means organizing similar classes under a module. In other words, we'll use modules to group related classes. Therein lies the first advantage of using modules for namespacing. It becomes easy for us to recognize related classes in our code. The second advantage is it reduces the likelihood of our classes colliding with other similarly named classes in our codebase. Here's how we do it:
```ruby
module Mammal
  class Dog
    def speak(sound)
      p "dog: #{sound}"
    end
  end

  class Cat
    def say_name(name)
      p "cat: #{name}"
    end
  end
end

buddy = Mammal::Dog.new
kitty = Mammal::Cat.new

buddy.speak("Arf") #=> "dog: Arf"
kitty.say_name("kitty kitty") #=> "cat: kitty kitty"
```
Notice how we call classes in a module by appending the class name to the module name with two colons(`::`).

The second use case for modules we'll look at is using modules as a **container** for methods, called module methods. This involves using modules to house other methods. This is very useful for methods that seem out of place within your code. Let's use our `Mammal` module to demonstrate:
```ruby
module Mammal
	...
	def self.some_out_of_place_method(num)
		num ** 2
	end
end
```

Defining methods this way within a module means we can call them directly from the module:
```ruby
value = Mammal.some_out_of_place_method(2)
```

We can also call such methods by doing:
```ruby
value = Mammal::some_out_of_place_method(2)
```

## Private, Protected, and Public

The last thing we want to cover is something that's actually quite simple, but necessary; Method Access Control. Access Control is a concept that exists in a number of programming languages, including Ruby. It is generally implemented through the use of _access modifiers_. The purpose of access modifiers is to allow or restrict access to a particular thing. In Ruby, the things that we are concerned with restricting access to are the methods defined in a class. In a Ruby context, therefore, you'll commonly see this concept referred to as **Method Access Control**.

The way that Method Access Control is implemented in Ruby is through the use of the `public`, `private`, and `protected` access modifiers. Right now, all the methods in our `GoodDog` class are public methods. A **public method** is a method that is available to anyone who knows either the class name or the object's name. These methods are readily available for the rest of the program to use and comprise the class's _interface_ (that's how other classes and objects will interact with this class and its objects).

Sometimes you'll have methods that are doing work in the class but don't need to be available to the rest of the program. These methods can be defined as **private**. How do we define private methods? We use the `private` method call in our program and anything below it is private (unless another method, like `protected`, is called after it to negate it).

In our `GoodDog` class we have one operation that takes place that we could move into a private method. When we initialize an object, we calculate the dog's age in Dog years. Let's refactor this logic into a method and make it private so nothing outside of the class can use it.

```ruby
class GoodDog
  DOG_YEARS = 7
  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age = a
  end

  private

  def human_years
    age * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
sparky.human_years
#=> private method `human_years' called for #<GoodDog:0x00007055dc3b04c8 @name="Sparky", @age=4> (NoMethodError)
```

We can an error because we have made the `human_years` method private by placing it under the `private` method. What is it good for, then, if we can't call it? `private` methods are only accessible from other methods in the class. For example, given the above code, the following would be allowed:
```ruby
class GoodDog
  DOG_YEARS = 7
  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age = a
  end

  def public_disclosure
    "#{self.name} in human years is #{self.human_years}"
  end

  private

  def human_years
    age * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
puts sparky.public_disclosure
#=> Sparky in human years is 28
```

Note that in this case, we can _not_ use `self.human_years`, because the `human_years` method is private. Remember that `self.human_years` is equivalent to `sparky.human_years`, which is not allowed for private methods. Therefore, we have to just use `human_years`. In summary, private methods are not accessible outside of the class definition at all, and are only accessible from inside the class when called without `self`.
But!

> As of Ruby 2.7, it is now legal to call private methods with a literal `self` as the caller. Note that this does **not** mean that we can call a private method with any other object, not even one of the same type. We can only call a private method with the current object.


Public and private methods are most common, but in some less common situations, we'll want an in-between approach. For this, we can use the `protected` method to create **protected** methods. Protected methods are similar to private methods in that they cannot be invoked outside the class. The main difference between them is that protected methods allow access between class instances, while private methods do not.

```ruby
class Person
  def initialize(age)
    @age = age
  end

  def older?(other_person)
    age > other_person.age
  end

  protected

  attr_reader :age
end

malory = Person.new(64)
sterling = Person.new(42)

puts malory.older?(sterling) #=> true
puts sterling.older?(malory) #=> false

puts malory.age #=> protected method `age' called for #<Person:0x00007464775e1fc8 @age=64> (NoMethodError)
```

The above code shows us that like private methods, protected methods cannot be invoked from outside of the class. However, unlike private methods, other instances of the class (or subclass) can also invoke the method. This allows for controlled access, but wider access between objects of the same class type.

## Accidental Method Overriding

It’s important to remember that every class you create inherently subclasses from [class Object](https://docs.ruby-lang.org/en/3.2/Object.html). The `Object` class is built into Ruby and comes with many critical methods.

```ruby
class Parent
  def say_hi
    p "Hi from Parent"
  end
end

puts Parent.superclass #=> Object
```

This means that methods defined in the `Object` class are available in *all classes*.

Further, recall that through the magic of inheritance, a subclass can override a superclass's method.
```ruby
class Child < Parent
  def say_hi
    p "Hi from Child"
  end
end

child = Child.new
child.say_hi #=> "Hi from Child"
```

This means that, if you accidentally override a method that was originally defined in the `Object` class, it can have far-reaching effects on your code. For example, `send` is an instance method that all classes inherit from `Object`. If you defined a new `send` instance method in your class, all objects of your class will call your custom `send` method, instead of the one in class `Object`, which is probably the one they mean to call. Object `send` serves as a way to call a method by passing it a symbol or a string which represents the method you want to call. The next couple of arguments will represent the method's arguments, if any. Let's see how [send](https://docs.ruby-lang.org/en/3.2/Object.html#method-i-send) normally works by making use of our `Child` class:

```ruby
son = Child.new
son.send :say_hi #=> "Hi from Child"
```

Let's see what happens when we define a `send` method in our `Child` class and try to invoke `Object`'s `send` method.
```ruby
class Child < Parent
  def say_hi
    p "Hi from Child"
  end

  def send
    p "HAHA!"
  end
end

son = Child.new
son.send :say_hi #=> wrong number of arguments (given 1, expected 0) (ArgumentError)
```

We'd expect it to put like `"Hi from Child"` but upon running the code, we get an error.

In our example, we're passing `send` one argument even though our overridden `send` method does not take any arguments. Let's take a look at another example by exploring Object's `instance_of?` method. What this handy method does is to return `true` if an object is an instance of a given class and `false` otherwise. Let's see it in action:
```ruby
class Child < Parent
  def say_hi
    p "Hi from Child"
  end

  def instance_of?
    p "Not an instance man"
  end
end

son = Child.new
son.instance_of? Child #=> wrong number of arguments (given 1, expected 0) (ArgumentError)
```

That said, one `Object` instance method that's easily overridden without any major side-effect is the `to_s` method. You'll normally want to do this when you want a different string representation of an object. Overall, it’s important to familiarize yourself with some of the common `Object` methods and make sure to not accidentally override them as this can have devastating consequences for your application.


