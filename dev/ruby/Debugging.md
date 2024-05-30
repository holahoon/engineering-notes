## Reading the stack trace

When Ruby program crashes after encountering a runtime error or exception, it produces a wall of text known as a **stack trace**.
This stack trace prints each line of code in your program that was executed before it crashed. The very first line of the stack trace will generally provide the most useful information about the error.
Also, note that in Ruby, [errors](https://docs.ruby-lang.org/en/3.3/Exception.html) are also objects.

## Debugging with puts

The easiest and the quickest way to confirm your assumptions while debugging is by using `puts` statements to output the return value of variables, methods, calculations, iterations, or even entire lines of code to the terminal.

## Debugging with puts and nil

Using `puts` is a great way to debug, but there's a **HUGE** caveat with using it: calling `puts` on anything that is `nil` or an empty string or collection will just print a blank line in the terminal 🤔.

This is one instance where using `p` will yield more information. `p` is a combination of `puts` and [#inspect](https://docs.ruby-lang.org/en/3.3/Object.html#method-i-inspect), the latter of which essentially prints a string representation of whatever it is called on.
```ruby
puts "Using puts:" #=> Using puts:
puts [] #=> 

p "Using p:" #=> Using p:
p [] #=> []
```

## Debug with Pry-byebug

[pry](https://github.com/pry/pry) is a Ruby gem that provides an interactive [REPL](https://www.rubyguides.com/2018/12/what-is-a-repl-in-ruby/) while your program is running.
To use Pry-byebug, you’ll first need to install it in your terminal by running `gem install pry-byebug`. You can then make it available in your program by requiring it at the top of your file with `require 'pry-byebug'`. Finally, to use Pry-byebug, you just need to call `binding.pry` at any point in your program.
When your code executes and gets to `binding.pry`, it will open an IRB-like session in your terminal. You can then use that session to check the values of anything within the scope of where you included `binding.pry`. However, keep in mind that any code written _after_ the `binding.pry` statement will not have been evaluated during the Pry session.
