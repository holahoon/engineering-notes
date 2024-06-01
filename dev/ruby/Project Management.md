For Ruby projects, the rule of thumbs are:
- One class per file. Every time you create a new class, you should create a new file for it to live in.
- It is convention to put all your Ruby files into a lib directory.
```
project_name
├── lib
│   └── lovely_file_of_yours.rb
└── main.rb
```

### Making use of multiple files

If you are to split your code across multiple files, you first will need to know how to make sure code from one file can be used in another file. Let’s consider this file structure:
```
├── lib
│   ├── sort
│   │   ├── bogo_sort.rb
│   │   ├── bubble_sort.rb
│   │   └── merge_sort.rb
│   └── sort.rb
└── main.rb
```

There are two main ways to do that: `require_relative` and `require`.

#### require_relative

```ruby
# You're in the root of the project, the directory that holds main.rb

# main.rb
require_relative 'lib/sort'

# sort.rb
require_relative 'sort/bubble_sort'
require_relative 'sort/bogo_sort'
require_relative 'sort/merge_sort'
```

Let’s start with how the docs define its functionality:

> require_relative(string) → true or false Ruby tries to load the library named `string` relative to the directory containing the requiring file. If the file does not exist a `LoadError` is raised. Returns `true` if the file was loaded and `false` if the file was already loaded before.

The important part here is _relative to the directory containing the requiring file_. This means that no matter where you execute the code from, `require_relative` looks for the file specified from the point of view of the file it has been written in. So `main.rb` is simply going to `lib` to find `sort` (the .rb is implicit), and `sort.rb` is going to `sort` to find those three different sorts. Simple enough, isn’t it?

#### require

`require` is a bit trickier. The doc describes:
> If the feature is an absolute path (e.g. starts with `'/'`), the feature will be loaded directly using the absolute path. If the feature is an explicit relative path (e.g. starts with `'./'` or `'../'`), the feature will be loaded using the relative path from the current directory. Otherwise, the feature will be searched for in the library directories listed in the `$LOAD_PATH`.

The absolute path bit seems self-explanatory. When you use a relative path the difference between using a relative path with `require` and doing `require_relative` is that `require`’s relative paths are resolved from the point of view of the directory you are running your code from. Let’s change our example:
```ruby
# You're in the root of the project, the directory that holds main.rb

# main.rb
require 'lib/sort'

# sort.rb
require 'sort/bubble_sort'
require 'sort/bogo_sort'
require 'sort/merge_sort'
```

Ah. Of course - an error - it can’t find `lib/sort`! Those are not relative paths… Fancy schmancy `require_relative` and its implicitly assuming the paths are relative!

```ruby
# You're in the root of the project, the directory that holds main.rb

# main.rb
require './lib/sort'

# sort.rb
require './sort/bubble_sort'
require './sort/bogo_sort'
require './sort/merge_sort'
```

Now it says it can’t find `./sort/bubble_sort`! This is because it is not looking for it from the point of view of `sort.rb` but from the point of view of `main.rb`.

What about the `$LOAD_PATH` part?

```ruby
# You're in the root of the project, the directory that holds main.rb

# main.rb
require 'csv'

require_relative 'lib/sort'
```

`require 'csv'` is going to look for a `csv.rb` in the Ruby’s `$LOAD_PATH` global variable which by default contains the Ruby standard library. There are other file extensions it might look for, but this is not important at this point - just remember that the `require`s look for some extensions like `.rb` without the need to declare them explicitly. In addition to that, if it doesn’t find that file in `$LOAD_PATH` it is going to look through installed gems (more on those later) to see if the file is there.

Both of those approaches (`require` and `require_relative`) are going to execute the file, allowing you to use their contents. If you try to require something for the second time, nothing will happen, and the requires will return `false`.

Convention is that `require_relative` is used for your own code, while `require` is used for things outside of it, like gems that your app depend on.

Benefit of this approach is that you don’t need to hold all the code for part of your app in one file:
```ruby
# You're in the root of the project, the directory that holds main.rb

# This is your file structure:
├── lib
│    ├── flight.rb
│    ├── hotel.rb
│    └── airport.rb
└── main.rb

# lib/airport.rb
class Airport
  def introduce
    puts "I'm at the airport!"
  end
end

# lib/flight.rb
class Flight
  def introduce
     puts "I'm on the flight!"
  end
end

# lib/hotel.rb
class Hotel
  def introduce
     puts "I'm at the hotel!"
  end
end

# main.rb
require_relative 'lib/airport'
require_relative 'lib/flight'
require_relative 'lib/hotel'

Airport.new.introduce
#=> I'm at the airport!

Flight.new.introduce
#=> I'm on the flight!

Hotel.new.introduce
#=> I'm at the hotel!
```

So instead of defining both the `Flight` and `Hotel` classes inside `airport.rb`, we can do that in separate files. It is customary to require all the files in your topmost file, like `main.rb` here. This allows everyone to just get hold of `main.rb` and they get the entirety of your code where they need it. Depending on their needs, they would use an appropriate way of loading that file.

Another thing to keep in mind is that local variables do not get loaded, so if your `airport.rb` had a local variable `coolest_airports`, trying to access it in `main.rb` would raise an error. Constants do get loaded however, so you can access those.

Something important to keep in mind is that all required code is put into the same namespace. This means that if you have the same names for methods, modules, classes and so on they will be added together in order they were required. For example, let’s say you and your friend have used the same method name and you’re trying to use their code and yours:

```ruby
# all files are in the same directory for simplicity's sake

# not_so_green.rb
def food_opinion(food)
  `#{food} is awesome!`
end

# scheals.rb
def food_opinion(food)
  `#{food} is awful!`
end

# main.rb
require_relative 'not_so_green'
require_relative 'scheals'

puts food_opinion('Cereal')
#=> Cereal is awful!
# Since food_opinion is defined twice, the last definition wins out.
```

To make sure code doesn’t get overwritten, Rubyists wrap their code in modules which give them the benefits of a namespace:
```ruby
# all files are in the same directory for simplicity's sake

# not_so_green.rb
module NotSoGreen
  def self.food_opinion(food)
    `#{food} is awesome!`
  end
end
# scheals.rb
module Scheals
  def self.food_opinion(food)
    `#{food} is awful!`
  end
end
# main.rb
require_relative 'not_so_green'
require_relative 'scheals'

puts NotSoGreen.food_opinion('Cereal')
#=> Cereal is awesome!
puts Scheals.food_opinion('Marmite')
#=> Marmite is awful!
puts food_opinion('Cereal')
#=> Errors out - there's no longer a free floating foo method to use.
```

### Gem and you

Gems are packages containing Ruby utility libraries that someone wrote. Some of those gems are part of the Ruby standard library, but most require installing independently.

If you use a gem then you call such a gem a dependency - your code depends on that gem to function properly. Some dependencies are only used in particular contexts; for example, you can have a set of gems used only in a development or test environment.

RubyGems has been part of Ruby standard library since version 1.9 and is used to get those amazing gems onto your computer. Remember that bit about `require` going through installed gems to potentially find a file you’re looking for? That’s the work of RubyGems. Another cool part about it? It is a gem itself! Let’s give it a try.

Create a new Ruby file `main.rb` in a directory called `colorful`:
```ruby
require 'colorize'

puts 'Red goes faster!'.colorize(:red)

puts "I'm blue da ba dee da ba di!".colorize(:blue)

puts "It ain't easy bein' green...".colorize(:green)
```

You’re probably itching to see all those colors, so run your file with `ruby main.rb` to see them… or rather, a LoadError. Right - you need to install that gem first! Do that with `gem install colorize` and you’ll see RubyGems in action. Your system now has access to the `Colorize` gem!

Wait, _your system_ - what about others who would like to use your code? Yeah, they would also need to `gem install` it - no big deal.

But what if you have dozens of gems? How do you ensure that the versions you use are the same version others download? This sounds rather tedious. Enter: Bundler. It’s another gem, part of RubyGems, but released independently.

Bundler allows you to declare what gems your project needs - down to their version. As for others, Bundler allows them to take that declaration, a simple file called `Gemfile`, and use it to install those gems in a quick `bundle install`.

Since gem installs are global, you need a way to run only those particular gem versions that are declared in the `Gemfile`. You can do that by using `bundle exec` followed by a command you want to execute - most likely `bundle exec ruby foo.rb`.

Let’s make sure whoever wants to use our script can do so:
```ruby
bundle init # creates a default Gemfile in the current working directory
bundle add colorize # adds the colorize gem to the Gemfile and runs bundle install
```

This has created two files: `Gemfile` and `Gemfile.lock`.
```ruby
# Gemfile

# frozen_string_literal: true

source "https://rubygems.org"

# gem "rails"

gem "colorize", "~> 1.1"

# Gemfile.lock
GEM
  remote: https://rubygems.org/
  specs:
    colorize (1.1.0)

PLATFORMS
  ruby
  x86_64-linux # This might be different for you if you're using a different CPU and OS.

DEPENDENCIES
  colorize (~> 1.1)

BUNDLED WITH
   2.5.4
```

There’s not much in those, but as you can see, the `Gemfile` has information on where to get the gems from and what gems are required.

The `"~> 1.1"` is a version constraint, particularly a pessimistic constraint. It relies on semantic versioning.

- The first number is the **major** version
- The second is the **minor** version
- the third, if it exists, is the **patch** number

Major versions can break things from previous versions - for example, changing method names. Minor versions can add and change things but can’t break anything. Patches happen when you introduce bug fixes that don’t break anything.

So, if people behind a gem maintain it in line with semantic versioning, you can rely on this pessimistic constraint never letting your project have a gem version that could potentially break your app - it is equivalent to `gem "colorize", ">= 1.1", "<2.0"`.

`Gemfile.lock` has information on what was the last environment that should be able to run your app. Bundler will use it to install the same versions of gems even if `Gemfile` could potentially allow for newer versions to be installed.

### .ruby-version

There’s another important thing to give to folks that will run your code: the target Ruby version of your project. You can do it easily by running `rbenv local 3.2.2` as it creates a `.ruby-version` file with the version declared.

Many other tools recognize this to figure out what Ruby version your project is running - for example, `rbenv` will no longer use the `global` Ruby version and the `Ruby LSP` VSCode extension will also change its behavior.


Here are some really handy articles about [GEMS](https://guides.rubygems.org/rubygems-basics/).


