Introduced in Ruby 2.7, pattern matching uses specified patterns to match against some data. If the data conforms to the pattern, there is a match and the data is deconstructed accordingly. If there is no match, you can either supply a default value to return or else a `NoMatchingPatternError` is raised.

With Ruby 3.1, most parts of the pattern matching syntax are no longer considered experimental, so it is now worth ensuring you are familiar with the basics. The syntax can feel a little clunky at first, but there are times it can definitely simplify Ruby code. There are a couple of new patterns with Ruby 3 which we’ll introduce at the end.

## Basics

The basic format for a pattern match is a case statement. This is not too different from the case statement you are already familiar with for matching conditions in Ruby, except now instead of `when`, we use `in`. If your use case is very basic, you will find there is no difference between using either `in` or `when` as the below example illustrates.
`when`
```ruby
grade = "C"

case grade
  when "A" then puts "Amazing effort"
  when "B" then puts "Good work"
  when "C" then puts "Well done"
  when "D" then puts "room for improvement"
else puts "See me"
end
#=> Well done
```
`in`
```ruby
grade = "C"

case grade
  in "A" then puts "Amazing effort"
  in "B" then puts "Good work"
  in "C" then puts "Well done"
  in "D" then puts "room for improvement"
else puts "See me"
end
#=> Well done
```

The second format is one line syntax using the trusty hash rocket that will be familiar to you from hashes:
```ruby
login = {username: "hornby", password: "123"}

login => {username: username}

puts "Username: #{username}" #=> Username: hornby
```

Just note that we can use the hash rocket `=>` to match against some kind of structure. The second format is still considered experimental so the examples that follow will use the case statement format.

The case/in format is best used when there are multiple conditionals you could possibly match against and you need to check against all of them. The hash rocket syntax is best used when the data structure you are matching against is known, such as the login data example we used above.

## Patterns

There are multiple ways of potentially matching against an input. Patterns can be:
- Any Ruby object which is matched using `===`. The Object Pattern.
- A variable capture / Variable Pattern
- An As Pattern
- An Alternative Pattern
- A Guard Condition
- An Array Pattern
- A Hash Pattern

You can use the above patterns while also having the following experimental additions.
- Rightward Assignment
- A Find Pattern

Patterns can also be matched using many of the patterns above together. For example, you may have an array inside a hash, so you could use the hash and array patterns.

## Return values

There are two possible outcomes for a pattern match statement - either there is a match or there is no match. If there is a match, it will return the last evaluated value in the body of the matching branch. If there are no matches, the pattern matching statement will return `NoMatchingPatternError`.
```ruby
grade = "C"

result = case grade
  in "A" then 1
  in "B" then 2
  in "C" then 3
  else 0
end

#=> 3
```

## Object Pattern match

Any object can be used in a pattern match. It is matched using `===` to compare the two objects and is the same basis for matches in the case/when format. This pattern is usually used within other patterns, such as the array pattern.

The grade example provided earlier was one such use of the Ruby object pattern match. On each check, Ruby compared the grade with the specified letter. When there was a match, it executed the then clause of that match. `'A' === 'A' #=> true`.

Because of this we can also check against data types.

```ruby
input = 3

case input
  in String then puts "input is of type String"
  in Integer then puts "input is of type Integer"
end

#=> input is of type Integer
```

It’s important to note here that Ruby places the pattern to match on the left of the comparison in `===`. If it didn’t then there would be no match. `3 === Integer` is false whereas `Integer === 3` is true. That is because of how Ruby implements the `===` method on Integer vs on instances of Integer. On Integer, `===` will check that the operand on the right of the comparison is of type Integer. On an instance of an integer, `===` will check they hold the same value. `3 === 3` is true.
```ruby
3 === Integer #=> false
Integer === 3 #=> true
```

With Ruby pattern matching, you can match against the following literal types.
- Booleans
- nil
- Numbers
- Strings
- Symbols
- Arrays
- Hashes
- Ranges
- Regular Expressions
- Procs

## Variable pattern

The variable pattern binds a variable or variables to the values that match the pattern.
```ruby
age = 15

case age
  in a
    puts a
end
#=> 15
```
This example isn't useful in itself, but lays the foundation for use in other patterns as we'll see.
Something to take note of is that the variable pattern always binds the value to the variable. Be careful if there is another variable with the same name in the outer scope.
```ruby
a = 5

case 1
  in a
    a
end

puts a #=> 1
```

Above, you might have thought you were comparing the value held in the initial `a` variable against the value `1` from the case statement. What actually happened is a variable assignment pattern match. To avoid this, Ruby provides the pin operator `^` which instead matches against a variable of that name.
```ruby
a = 5

case 1
  in ^a
    :no_match
end

puts a #=> 1: 5 === 1 does not return true (NoMatchingPatternError)
```

## As pattern match

The as pattern is similar to the variable pattern but can be used to manage more complex assignments. This will make most sense when using Arrays and Hashes.
```ruby
case 3
  in 3 => a
    puts a #=> 3
end
```

It uses the hash rocket in the same way the one line pattern match does. Look out for the examples in later sections for clarification on when this pattern could be useful.

## Alternative pattern match

The alternative pattern allows you to check if multiple options match the input.
```ruby
case 0
  in 0 | 1 | 2
    puts :match #=> match
end
```

There isn’t much more to it. The same rules apply as with any other pattern. So you can use the `^` operator if you are using variables to compare.

## Guard conditions

This isn't a pattern per se, but a way to make sure the pattern is only matched if the guard condition holds true.
```ruby
some_other_value = true

case 0
  in 0 if some_other_value
    puts :match
end
```

You can use any expression in the condition that can evaluate to true or false. You can also use `unless` instead of `if` if it suits you.

## Array pattern match

Matching against arrays can be done in a few different ways. At its most basic, you can match against the exact elements in the array.
```ruby
arr = [1, 2]

case arr
  in [1, 2] then puts :match #=> match
  in [3, 4] then puts :no_match
end
```

That works using the `===` operator from the object pattern match so would work using case/when. That's no fun, so let's ramp it up a little bit.
```ruby
arr = [1, 2]

case arr
  in [Integer, Integer] #=> match
    puts :match
  in [String, String]
    puts :no_match
end
```

What happens if we have more elements in the array?
```ruby
arr = [1, 2, 3]

case arr
  in [Integer, Integer]
    puts :no_match
end
#=> [1, 2, 3]: [1, 2, 3] length mismatch (given 3, expected 2) (NoMatchingPatternError)
```

An error! Ruby appears to only match against arrays with the same number of elements. What if you want to match against only part of an array? Use the trusty splat `*`.
```ruby
arr = [1, 2, 3]

case arr
  in [Integer, Integer, *]
    puts :match
end
#=> match
```

You can place the splat anywhere in an array to match against multiple entries.
```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8]

case arr
  in [Integer, Integer, *, Integer]
    puts :match
end
#=> match
```
Here we are checking only the first and last two elements are Integers.

You can also mix and match between checking actual values and types:
```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8]

case arr
  in [Integer, Integer, *, 7, 8]
    puts :match
end
#=> match
```

If you want to scoop up the values of the array matched against the splat, you can use the variable pattern.
```ruby
arr = [1, 2, 3, 4]

case arr
  in [1, 2, *tail]
    puts tail
end
#=> 3
#=> 4
```

If you don't care about some values in the array and are happy to match against any value for that index, you can use `_`.
```ruby
arr = [1, 2, 3, 4]

case arr
  in [_, _, 3, 4]
    puts :match
end
#=> match
```

Let's say you want to match against an array of two numbers, but only if they aren't the same number. You can use a guard clause.
```ruby
arr = [1, 2]

case arr
  in [a, b] unless a == b
    puts :match
end
#=> match
```

You can even match against nested arrays.
```ruby
arr = [1, 2, [3, 4]]

case arr
  in [_, _, [3, 4]]
    puts :match
end
#=> match
```

This is where the real power of pattern matching shines: traversing deeply nested structures for matches.

You can incorporate the variable pattern to bind matching values to variables to use later.
```ruby
arr = [1, 2, 3, 4, 5]

case arr
  in [1, 2, 3, a, b]
    puts a
    puts b
end
#=> 4
#=> 5
```

Let's say you have a nested array and you want to match against both the nested array and some individual parts of it. This is where the as pattern can be used.
```ruby
arr = [1, 2, 3, [4, 5], [6]]

case arr
  in [1, 2, 3, [a, 5] => arr, [b] => arr_b]
  puts a #=> 4
  p arr #=> [4, 5]
  
  puts b #=> 6
  p arr_b #=> [6]
end
```

Be careful when with variable reassignment as we discussed earlier.
```ruby
arr = [1, 2, 3]

case [2, 4, 6]
  in arr
  :match
end

p arr #=> [2, 4, 6]
```

One last point to note is that when matching an array, the `[]` are optional on the outer array.
```ruby
arr = [1, 2, 3, 4]

case arr
  in 1, 2, 3, 4
  puts :match
end
#=> match
```

## Hash pattern matching




