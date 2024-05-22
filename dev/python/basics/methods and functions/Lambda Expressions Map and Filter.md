
### `map()`
`map()` function returns a map object(which is an iterator) of the results after applying the given function to each item of a given iterable (list, tuple, etc.)

```python
def double(num):
	return num * 2

my_nums = [1, 2, 3, 4, 5]

# option 1.
for item in map(double, my_nums):
	print(item)
# 2
# 4
# 6
# 8
# 10

# options 2.
list(map(double, my_nums))
# [2, 4, 6, 8, 10]
```

### `filter()`
The `filter()` method filters the given sequence with the help of a function that tests each element in the sequence to be true or not.

```python
def check_even(num):
    return num % 2 == 0

my_nums = [1, 2, 3, 4, 5, 6]

list(filter(check_even, my_nums)) # [2, 4, 6]
```

## Lambda Expressions

A lambda function in Python is a small anonymous function.

We would typically define a function:
```python
def square(num):
	return num ** 2
```

But, with the help of lambda expression, there's no need to assign to a name. Basically, lambdas are small throwaway functions you pass to something else immediately upon creation.
Lambda expressions:
```python
lambda num: num ** 2
```

We can use lambda expressions with `map()` or `filter()`

With `map()`:
```python
my_nums = [1, 2, 3, 4, 5, 6]
list(map(lambda num: num ** 2), my_nums)
# [1, 4, 9, 16, 25, 36]
```
Keep in mind that we can definitely assign this lambda function to a variable, but this defeats the purpose of using a lambda expression.

With `filter()`:
```python
list(filter(lambda num: num % 2 == 0, my_nums))
# [2, 4, 6]
```

We can do other easy stuffs with it:
```python
names = ['David', 'Sohyeon', 'Jinho']
list(map(lambda letter: letter[::-1], names))
# ['divaD', 'noeyhoS', 'ohniJ']
```

