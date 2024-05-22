## `*args`

We normally define positional arguments:
```python
def myfunc(a, b):
    return sum((a, b)) * 0.05

myfunc(40, 60) # 5.0
```
This will return `5.0` as a result.

There are ways to pass in arbitrary number of arguments using `*args`:
```python
def myfunc(*args):
	return sum(args) * 0.05

myfunc(40, 60, 100, 200) # 20.0
```
This is pretty much the same thing as `...args` in JavaScript.
Cool thing in Python, `*args` can be named something else. We can name it like `*spam` as long as it it followed by the asterisk. But it is a convention to name it as `args`.

Keep in mind that arbitrary arguments return a tuple:
```python
def myfunc(*args):
	return args

myfunc(10, 20, 30, 40) # (10, 20, 30, 40)
```

## `**kwargs`

It stands for arbitrary keyword arguments. It is pretty much just a dictionary.
```python
def myfunc(**kwargs):
    if 'fruit' in kwargs:
        print('My fruit of choice is {}'.format(kwargs['fruit']))
    else:
        print('I did not find any fruit here')

myfunc(fruit = 'apple', veggi = 'lettuce') # My fruit of choice is apple
```

If we print out the keyword arguments, it looks like a dictionary:
```python
def myfunc(**kwargs):
	print(kwrags)

myfunc(a = 1, b = 'BB', c = True # {'a': 1, 'b': 'BB', 'c': True}
```

We can combine `*args` and `**kwargs`:
```python
def myfunc(*args, **kwargs):
    print(args)
    print(kwargs)
    print('I would like {} {}'.format(args[0], kwargs['food']))

myfunc(10, 20, 30, fruit='orange', food='eggs', animal='dog')
# (10, 20, 30)
# {'fruit': 'orange', 'food': 'eggs', 'animal': 'dog'}
# I would like 10 eggs
```



