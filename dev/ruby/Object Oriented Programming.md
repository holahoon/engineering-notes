**Object Oriented Programming**, often referred to as **OOP**, is a programming [paradigm](https://en.wikipedia.org/wiki/Paradigm) that was created to deal with the growing complexity of large software systems. Programmers found out very early on that as applications grew in complexity and size, they became very difficult to maintain. One small change at any point in the program would trigger a ripple effect of errors due to dependencies throughout the entire program.

Programmers needed a way to create containers for data that could be changed and manipulated without affecting the entire program. They needed a way to section off areas of code that performed certain procedures so that their programs could become the interaction of many small parts, as opposed to one massive blob of dependency.

### Encapsulation

It describes the idea of bundling or combining the data and the operations that work on that data into a single entity, e.g., an object.

Encapsulation also hides functionality to make it unavailable to the rest of the code base. It is a form of data protection, so that data cannot be manipulated or changed without obvious intent. It is what defines the boundaries in your application and lets your code achieve new levels of complexity. Ruby, like many other OO languages, accomplishes this task by creating objects and exposing interfaces (i.e., methods) to interact with those objects.

In most OOP languages, encapsulation has a broader purpose. It also refers to restricting access to the state and certain behaviors; an object only exposes the data and behaviors that other parts of the application needs to work. In other words, objects expose a **public interface** for interacting with other objects and keep their implementation details hidden. Thus, other objects can't change the data of an object without going through the proper interface.

### Polymorphism

**Polymorphism** is the ability for different types of data to respond to a common interface. For instance, if we have a method that invokes the `move` method on its argument, we can pass the method any type of argument as long as the argument has a compatible `move` method. The object might represent a human, a cat, a jellyfish, or, conceivably, even a car or train. That is, it lets objects of different types respond to the same method invocation.

> "Poly" stands for "many" and "morph" stands for "forms". OOP gives us flexibility in using pre-written code for new purposes.

### Inheritance

The concept of **inheritance** is used in Ruby where a class inherits -- that is, acquires -- the behaviors of another class, referred to as the **superclass**. This gives Ruby programmers the power to define basic classes with large reusability and smaller **subclasses** for more fine-grained, detailed behaviors.

Another way to apply polymorphic structure to Ruby programs is to use a `Module`. Modules are similar to classes in that they contain shared behavior. However, you cannot create an object with a module. A module must be mixed in with a class using the `include` method invocation. This is called a **mixin**. After mixing in a module, the behaviors declared in that module are available to the class and its objects.

## What Are Objects?

Throughout the Ruby community you'll often hear the phrase, "In Ruby, everything is an object!". We've avoided this so far since objects are a more advanced topic and it's necessary to get a handle on basic Ruby syntax before you start thinking about objects.

> It's not even strictly true; not everything in Ruby is an object. However, anything that can be said to have a value **is** an object: that includes numbers, strings, arrays, and even classes and modules. However, there are a few things that are not objects: methods, blocks, and variables are three that stand out.

Objects are created from classes. Think of classes as molds and objects as the things you produce out of those molds. Individual objects will contain different information from other objects, yet they are instances of the same class. Here's an example of two objects of the `String` class:
```ruby
irb :001 > "hello".class => String
irb :002 > "world".class => String
```
 we use the `class` instance method to determine what the class is for each object. As you can see, everything we've been using, from strings to integers, are in fact objects, which are instantiated from a class.

## Classes Define Objects

Ruby defines the attributes and behaviors of its objects in **classes**. You can think of classes as basic outlines of what an object should be made of and what it should be able to do. To define a class, we use syntax similar to defining a method. We replace the `def` with `class` and use the CamelCase naming convention to create the name. We then use the reserved word `end` to finish the definition. Ruby file names should be in snake_case, and reflect the class name. Thus, in the below example, the file name is `good_dog.rb` and the class name is `GoodDog`.
```ruby
class GoodDog
# logic...
end

sparky = GoodDog.new
```

In the above example, we created an instance of our `GoodDog` class and stored it in the variable `sparky`. We now have an object. We say that `sparky` is an object or instance of class `GoodDog`. This entire workflow of creating a new object or instance from a class is called **instantiation**, so we can also say that we've instantiated an object called `sparky` from the class `GoodDog`. The terminology in OOP is something you'll eventually get used to, but the important fact here is that an object is returned by calling the class method `new`.

## Modules

As we mentioned earlier, modules are another way to achieve polymorphism in Ruby. A **module** is a collection of behaviors that is usable in other classes via **mixins**. A module is "mixed in" to a class using the `include` method invocation. Let's say we wanted our `GoodDog` class to have a `speak` method but we have other classes that we want to use a speak method with too. Here's how we'd do it.
```ruby
module Speak
	def speak(sound)
		puts sound
	end
end

class GoodDog
	include Speak
end

class HumanBeing
	include Speak
end

sparky = GoodDog.new
sparky.speak("Arf!") #=> Arf!

bob = HumanBeing.new
bob.speak("Hello!") #=> Hello!
```

Note that in the above example, both the `GoodDog` object, which we're calling `sparky`, as well as the `HumanBeing` object, which we're calling `bob`, have access to the `speak` instance method. This is possible through "mixing in" the module `Speak`. It's as if we copy-pasted the `speak` method into the `GoodDog` and `HumanBeing` classes.

## Method Lookup

When you call a method, how does Ruby know where to look for that method? Ruby has a distinct lookup path that it follows each time a method is called. Let's use our program from above to see what the method lookup path is for our `GoodDog` class. We can use the `ancestors` method on any class to find out the method lookup chain.

```ruby
module Speak
  def speak(sound)
    puts "#{sound}!"
  end
end

class GoodDog
  include Speak
end

puts GoodDog.ancestors
#=> GoodDog
#=> Speak
#=> Object
#=> Kernel
#=> BasicObject
```

The `Speak` module is placed right in between our custom classes (i.e., `GoodDog`) and the `Object` class that comes with Ruby. In [Inheritance](https://launchschool.com/books/oo_ruby/read/inheritance) you'll see how the method lookup chain works when working with both mixins and class inheritance.

# [[Classes and Objects 1]]





