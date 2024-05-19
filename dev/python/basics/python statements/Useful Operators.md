## Range Operator

```python
for num in range(5):
	print(num)
# 0
# 1
# 2
# 3
# 4
```
It creates a collection of numbers on the fly, but starting from 0 to not including the number.

We can also specify where to start, end and steps:
```python
for num in range(3, 10, 3):
    print(num)

# 3
# 6
# 9
```

We can also generate a list of numbers:
```python
list(range(5))
# [0, 1, 2, 3, 4]
```

## Enumerate Operator

It seems like Python doesn't provide index in a for loop. So we'd have to make it like:
```python
idx_count = 0

for letter in 'abced':
    print('At index {}, the letter is {}'.format(idx_count, letter))
    idx_count += 1
```

Python provides an operator called `enumerate`:
```python
word = 'abcde'

for letter in enumerate(word):
    print(letter)
    
# (0, 'a')
# (1, 'b')
# (2, 'c')
# (3, 'd')
# (4, 'e')
```
Notice how these are returned as a tuple. We can obviously unpack them:
```python
word = 'abcde'

for idx, letter in enumerate(word):
    print(idx, letter)

# 0 a
# 1 b
# 2 c
# 3 d
# 4 e
```

## Zip Operator

We can "zip" to lists together:
```python
mylist1 = [1, 2, 3, 4, 5]
mylist2 = ['a', 'b', 'c']

zip(mylist1, mylist2)
# <zip at 0x10c578bc0>
```
Well, this will return us the memory where this data is stored.
We can use `zip` in our for loop:
```python
for item1, item2 in zip(mylist1, mylist2):
    print(item1, item2)

# 1 a
# 2 b
# 3 c
```
Notice how i'm unpacking them by `item1` and `item2` because this returns us a tuple.
Also, notice that it only "joins" the shortest list and ignores any extras.

We can also create a list:
```python
list(zip(mylist1, mylist2))
# [(1, 'a'), (2, 'b'), (3, 'c')]
```

## In Operator

We can check if certain value exists in a list:
```python
'x' in ['x', 'y', 'z']
# True
```

It works on other iterable object types:
```python
'a' in 'a world'
# True
```

```python
'mykey' in {'mykey': 1234}
# True

dict = {'key1': 123, 'key2': 456}
456 in dict.keys()
# False
```

## Min & Max Operators

```python
mylist = [10, 20, 30, 40, 50, 100]

min(mylist)
# 10

max(mylist)
# 100
```

## Random library

Let's try importing `shuffle` function from the `random` library:
```python
from random import shuffle

mylist = [1, 2, 3, 4, 5, 6, 7]
shuffle(mylist)

# something like: [2, 6, 3, 1, 5, 4]
```
`shuffle` does not return anything because it is an in-place function(operates in-place on the list).

## Accept user input

```python
input('Enter a number here: ')
```
Note, `input` accepts any values as a string. We can also save the value in a variable.

We can transform this value into like an integer:
```python
result = int(input('Favorite number: '))
# user types in 30
print(type(result))
# int
```


