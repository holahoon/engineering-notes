
Many objects in Python are "iterable", meaning we can iterate over every element in the object.
Such as every element in a list or every character in a string.
We can use for loops to execute a block of code for every iteration.

```python
my_list = [1, 2, 3]
for item in my_list:
	print(item)
# 1
# 2
# 3
```

### Iterating over list of tuples

```python
my_list = [(1,2), (3,4), (5,6), (7,8)]
for item in my_list:
	print(item)
# (1, 2)
# (3, 4)
# (5, 6)
# (7, 8)
```

We can also do a "tuple unpacking" by making variable names:
```python
for (a, b) in my_list:
	print(b)
# 2
# 4
# 6
# 8
```
We can omit the parenthesis as well:
```python
for a, b in my_list:
	...
```

###  Iterating over dictionaries

It's crazy how Python lets you iterate over dictionaries 🤯

```python
dict = {'k1': 1, 'k2': 2, 'k3': 3}
for key in dict:
	print(key)
	
# k1
# k2
# k3
```
Notice how it returns key of the dictionary. Well, this isn't because I'm calling each a "key".

To get both the key and value of each element in a dictionary:
```python
for val in dict.items():
	print(val)

# ('k1', 1)
# ('k2', 2)
# ('k3', 3)
```
We can unpack the result:
```python
for key, val in dict.items():
	print(key)

# k1 1
# k2 2
# k3 3
```

What if we just want the value?
```python
for val in dict.values():
	print(val)

# 1
# 2
# 3
```

💡 Keep in mind that our dictionary looks ordered in the example, but dictionary in Python does not guarantee order.
