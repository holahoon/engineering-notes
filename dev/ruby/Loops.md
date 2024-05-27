## Loop
The `loop` loop is an infinite loop that will keep going unless you specifically request for it to stop, using the `break` command.
```ruby
i = 0
loop do
  puts "i is #{i}"
  i += 1
  break if i == 10
end
```

## While loop
A `while` loop is similar to the `loop` loop except that you declare the condition that will break out of the loop up front.
```ruby
i = 0
while i < 10 do
  puts "i is #{i}"
  i += 1
end
```

You can also use the `while` loop to repeatedly ask a question of the user until they give the desired response:
```ruby
while gets.chomp != 'yes' do
	puts "Are we there yet?"
end
```
## Until loop
The `until` loop is the opposite of the `while` loop. A `while` loop continues for as long as the condition is true, whereas an `until` loop continues for as long as the condition is false. These two loops can therefore be used pretty much interchangeably. Ultimately, what your break condition is will determine which one is more readable.
As much as possible, you should avoid negating your logical expressions using `!` (not). First, it can be difficult to actually notice the exclamation point in your code. Second, using negation makes the logic more difficult to reason through and therefore makes your code more difficult to understand. These situations are where `until` shines.
```ruby
i = 0
until i >= 10 do
  puts "i is #{i}"
  i += 1
end
```

We can re-write using `until` to avoid the negation `!` that the above `while` loop had to use.
```ruby
until gets.chomp == 'yes' do
  puts "Are we there yet?"
end
```

## Ranges
What if we know exactly how many times we want our loop to run?
```ruby
(1..5) # inclusive range: 1, 2, 3, 4, 5
(1...5) # exclusive range: 1, 2, 3, 4

# We can make ranges of letters, too! 🤯
('a'..'d') # a, b, c, d
```
There are more in [ranges](https://docs.ruby-lang.org/en/3.3/Range.html)

## For loops
A `for` loop is used to iterate through a collection of information such as an array or range.
```ruby
for i in 0..5
  puts "#{i} zombies incoming!"
end
```

## Times loop
If you need to run a loop for a specified number of times, then look no further than the trusty `#times` loop. It works by iterating through a loop a specified number of times and even throws in the bonus of accessing the number it’s currently iterating through.
```ruby
5.times do
  puts "Hello, world!"
end

5.times do |number|
  puts "Alternative fact number #{number}"
end
```

## Upto and Downto loops
You can use these methods to iterate from a starting number either up to or down to another number, respectively.
```ruby
5.upto(10) { |num| print "#{num} " }     #=> 5 6 7 8 9 10

10.downto(5) { |num| print "#{num} " }   #=> 10 9 8 7 6 5
```

