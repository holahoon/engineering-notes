## If, elsif, else

This is the syntax of Ruby conditional logic:
```ruby
if statement_to_be_evaluated == true
	# do something here...
end
```

```ruby
if 1 < 2
	puts "1 is less than 2"
end
#=> "1 is less than 2"
```

We can even make it a one line
```ruby
puts "holy moly" if 1 < 2
```

Adding `else` and `elsif`
```ruby
if attack_by_land == true
	puts "release the goat"
elsif attack_by_sea == true
	puts "release the shark"
else
	puts "release Kevin the octopus"
end
```

## Boolean logic

### `==`
`==` (equals) returns `true` if the values compared are equal.

```ruby
5 == 5 #=> true
5 == 6 #=> false
```

### `!=`
`!=` (not equal) returns `true` if the values compared are not equal.

```ruby
5 != 7 #=> true
5 != 5 #=> false
```
### `>`
`>` (greater than) returns `true` if the value on the left of the operator is larger than the value on the right.

```ruby
7 > 5 #=> true
5 > 7 #=> false
```

### `<`
`<` (less than) returns `true` if the value on the left of the operator is smaller than the value on the right.

```ruby
5 < 7 #=> true
7 < 5 #=> false
```

### `>=`
`>=` (greater than or equal to) returns `true` if the value on the left of the operator is larger than or equal to the value on the right.

```ruby
7 >= 7 #=> true
7 >= 5 #=> true
```

### `<=`
`<=` (less than or equal to) returns `true` if the value on the left of the operator is smaller than or equal to the value on the right.

```ruby
5 <= 5 #=> true
5 <= 7 #=> true
```

### `eql?`
`#eql?` checks both the value type and the actual value it holds.

```ruby
5.eql?(5.0) #=> false; although they are the same value, one is an integer and the other is a float
5.eql?(5)   #=> true
```

### `equal?`
`#equal?` checks whether both values are the exact same object in memory. This can be slightly confusing because of the way computers store some values for efficiency. Two variables pointing to the same number will usually return `true`.

```ruby
a = 5
b = 5
a.equal?(b) #=> true
```
This expression is true because of the way computers store integers in memory. Although two different variables are holding the number 5, they point to the same object in memory. However, consider the next code example:

```ruby
a = "hello"
b = "hello"
a.equal?(b) #=> false
```
This happens because computers can’t store strings in the same efficient way they store numbers. Although the values of the variables are the same, the computer has created two separate string objects in memory.

### `<=>`
The **spaceship operator** returns one of three numerical values, unlike the comparison operators from above.

```ruby
5 <=> 10 #=> -1
10 <=> 10 #=> 0
10 <=> 5 #=> 1
```
- `-1` if the value on the left is less than the value on the right;
- `0` if the value on the left is equal to the value on the right; and
- `1` if the value on the left is greater than the value on the right.

## Logical Operators

Ruby has logical operators:
`&&` and `||`. The `&&` can also be replaced with `and`.
```ruby
if 1 < 2 && 5 < 6
	# do something
end

if 1 < 2 and 5 < 6
	# do something
end
```

Here's the or operator:
```ruby

if 1 < 2 || 5 < 6
	# do something
end

if 1 < 2 or 5 < 6
	# do something
end
```

The `!` operator
```ruby
if !false #=> true
if !(10 < 5) #=> true
```

## Case Statements

Case statements process each condition in turn, and if the condition returns `false`, it will move onto the next one until a match is found. An `else` clause can be provided to serve as a default if no match is found.

```ruby
grade = 'F'

did_i_pass = case grade #=> create a variable `did_i_pass` and assign the result of a call to case with the variable grade passed in
  when 'A' then "Hell yeah!"
  when 'D' then "Don't tell your mother."
  else "'YOU SHALL NOT PASS!' -Gandalf"
end

puts did_i_pass #=> "'YOU SHALL NOT PASS!' -Gandalf"
```

You can remove the `then` keyword and put more complex code to be executed on the next line.
```ruby
grade = 'F'

case grade
when 'A'
  puts "You are a genius"
  future_bank_acount_balance = 5_000_000
when 'D'
  puts "Better luck next time!"
  can_i_retire_soon = false
else
  puts "bruh..."
  fml = true
end

```

## Unless statements

An `unless` statement works in the opposite way as an `if` statement: it only processes the code in the block if the expression evaluates to `false`.
```ruby
age = 19
unless age < 18
	puts "Get a job"
end
#=> Get a job
```

You can also write on one line, or add `else` clause:
```ruby
age = 19
puts "Welcome to a life of debt." unless age < 18

unless age < 18
  puts "Down with that sort of thing."
else
  puts "Careful now!"
end
```

You should use an `unless` statement when you want to **not** do something if a condition is `true`, because it can make your code more readable than using `if !true`.

## Ternary operator

The ternary operator is a one-line `if...else` statement that can make your code much more concise.

Its syntax is `conditional statement ? <execute if true> : <execute if false>`. You can assign the return value of the expression to a variable.

```ruby
age = 19

response = age < 18 ? 'child' : 'adult'
puts response #=> adult
```
