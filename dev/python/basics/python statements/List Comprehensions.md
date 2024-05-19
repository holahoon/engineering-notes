
List Comprehensions are a unique way of quickly creating a list.
For loop along with `.append()` to create a list is okay as is, but List Comprehensions are a good alternative.

Here's the for loop way of creating a list:
```python
mystring = 'hello'
mylist = []
for letter in mystring:
    mylist.append(letter)
mylist
# ['h', 'e', 'l', 'l', 'o']
```

Here's the List Comprehension way:
```python
mylist = [letter for letter in mystring]
# ['h', 'e', 'l', 'l', 'o']
```

We can also create a list of numbers:
```python
numlist = [n for n in range(0, 11)]
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
We can also square root all the numbers:
```python
sqnum = [n**2 for n in range(0, 10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

We can also conditionally create a list:
```python
eventsqnum = [n for n in range(0, 11) if n % 2 ==0]
# [0, 2, 4, 6, 8, 10]
```
We can even do more complex calculation:
```python
celcius = [0, 10, 20, 34.5]
fahrenheit = [((9 / 5) * temp + 32) for temp in celcius]
# [32.0, 50.0, 68.0, 94.1]

# This is essentially the same as:
fahrenheit = []
for temp in celcius:
	fahrenheit.append((9 / 5) * temp + 32)
```

We can also do a if and else:
```python
result = [x if x % 2 == 0 else 'ODD' for x in range(0, 11)]
# [0, 'ODD', 2, 'ODD', 4, 'ODD', 6, 'ODD', 8, 'ODD', 10]
```
It's a cool one-liner, but I don't like writing complex stuffs in one line due to readability.

## Nest Loops

```python
mylist = []
for x in [2, 4, 6]:
    for y in [1, 10, 100]:
        mylist.append(x * y)

# [2, 20, 200, 4, 40, 400, 6, 60, 600]
```

Here's the List Comprehension:
```python
mylist = [x * y for x in [2, 4, 6] for y in [1, 10, 100]]
```
Well obviously, we sacrifice readability.
