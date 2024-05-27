
Ruby is a strongly object-oriented language, which means that absolutely everything in Ruby is an object, even the most basic data types. We’ll start here with four of Ruby’s basic data types: numbers (integers and floats), strings, symbols, and Booleans (`true`, `false`, and `nil`).

## Integers and floats

**Integers** are whole numbers, and **Floats** are numbers that contain a decimal point.

🤔 It's important to keep in mind that when doing arithmetic with two integers in Ruby, *the result will always be an integer*.
```ruby
17 / 5
#=> 3, not 3.4
```

To obtain an accurate answer, just replace one of the integers in the expression with a float:
```ruby
17 / 5.0
#=> 3.4
```

### Converting number types

🤔 Keep in mind the, Ruby doesn't do any rounding in this conversion.
```ruby
# To convert an integer to a float:
13.to_f #=> 13.0

# To convert a float to an integer:
13.0.to_i #=> 13
12.4.to_i #=> 12
```

### Some useful number methods

#### even?
```ruby
6.even? #=> true
7.even? #=> false
```

#### odd?
```ruby
4.odd? #=> false
4.odd? #=> true
```

## Strings

Strings can be formed with either double `""` or single`''` quotation marks, also known as _string literals_. They are pretty similar, but there are some differences. Specifically, string interpolation and the escape characters both only work inside double quotation marks, not single quotation marks.

```ruby
# With the plus operator:
"Welcome " + "to " + "Korea!" #=> "Welcome to Korea!"

# With the shovel operator:
"Welcome " << "to " << "Korea!" #=> "Welcome to Korea!"

# With the concat method:
"Welcome ".concat("to ").concat("Korea!") #=> "Welcome to Korea!"
```

### Substrings

You can access strings inside strings. Stringception.
```ruby
# string[index]
"hello"[0]      #=> "h"
"hello"[-1]     #=> "o"

# string[range]
"hello"[0..1]   #=> "he"

# string[start, end]
"hello"[0, 4]   #=> "hell"
```
There's more on the [docs](https://docs.ruby-lang.org/en/3.3/String.html#class-String-label-String+Slices)

### Escape characters

Escape characters allow you to type in representations of whitespace characters and to include quotation marks inside your string without accidentally ending it. As a reminder, escape characters only work inside double quotation marks.
```ruby
\\  #=> Need a backslash in your string?
\b  #=> Backspace
\r  #=> Carriage return, for those of you that love typewriters
\n  #=> Newline. You'll likely use this one the most.
\s  #=> Space
\t  #=> Tab
\"  #=> Double quotation mark
\'  #=> Single quotation mark
```

### Interpolation
String interpolation allows you to evaluate a string that contains placeholder variables.
🤔 Make sure to use double quotes!
```ruby
name = "DK"

puts "Hello, #{name}!" #=> "Hello, DK!"
puts 'Hello, #{name}!' #=> "Hello, #{name}!"
```

### Common string methods

Check the [docs](https://docs.ruby-lang.org/en/3.3/String.html) for more.
Here are some examples:

#### Capitalize
```ruby
"hello".capitalize #=> "Hello"
```
#### Include?
```ruby
"hello".include?("lo") #=> true
"dk".include?("z") #=> false
```
#### upcase
```ruby
"hello".upcase #=> "HELLO"
```
#### downcase
```ruby
"HeLLo".downcase #=> "hello"
```
#### empty?
```ruby
"hello".empty? #=> false
"".empty? #=> true
```
#### length
```ruby
"hello".length #=> 5
```
#### reverse
```ruby
"hello".reverse #=> "olleh"
```
#### split
```ruby
"hello world".split #=> ["hello", "world"]
"hello".split("") #=> ["h", "e", "l", "l", "o"]
```
#### strip
```ruby
"  hello, world    ".strip #=> "hello, world"
```

Here are some more cool stuffs:
```ruby
"he77o".sub("7", "l")           #=> "hel7o"

"he77o".gsub("7", "l")          #=> "hello"

"hello".insert(-1, " dude")     #=> "hello dude"

"hello world".delete("l")       #=> "heo word"

"!".prepend("hello, ", "world") #=> "hello, world!"
```

### Converting other objects to strings

Using the `to_s` method, you can convert pretty much anything to a string:
```ruby
5.to_s #=> "5"
nill.to_s #=> ""
:symbol.to_s #=> "symbol"
```

## Symbols

Symbols are an interesting twist on the idea of a string. The full explanation can be a bit long, but here’s the short version:

Strings can be changed, so every time a string is used, Ruby has to store it in memory even if an existing string with the same value already exists. Symbols, on the other hand, are stored in memory only once, making them faster in certain situations.

One common application where symbols are preferred over strings are the keys in hashes.

### Create a symbol

To create a symbol, put a colon at the beginning of some text:
```ruby
:name
:my_symbol
:"suprisingly, this is also a symbol"
```

### Symbols vs. Strings
```ruby
"string" == "string" #=> true

"string".object_id == "string".object_id #=> false

:my_symbol.object_id == :my_symbol.object_id #=> true
```
*Remember: in Ruby, everything is an object*

## Boolean

### True and false
literally `true` or `false`.

### Nill
In Ruby, `nill` represents "nothing". Everything in Ruby has a return value. When a piece of code doesn't have anything to return, it will return `nil`.


Here is a documentation that covers all the data types in Ruby
[launchschool](https://launchschool.com/books/ruby/read/basics)
