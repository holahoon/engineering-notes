
## Scope

### LEGB Rule:
This is a rule an order that Python is going to look.
- L : Local - Names assigned in any way within a function (`def` or `lambda`), and not declared global in that function.
- E : Enclosing function locals - Names in the local scope of any and all enclosing functions (`def` or `lambda`), from inner to outer.
- G : Global (module) - Names assigned at the top-level of a module file, or declared global in a `def` within the file.
- B : Built-in (Python) - Names preassigned in the built-in names module: `open`, `range`, `SyntaxError`, ...

```python
# GLOBAL
name = 'THIS IS A GLOBAL STRING'

def greet():
	# ENCLOSING
	name = 'DK'

	def hello():
		# LOCAL
		print('Hello' + name)

	hello()

greet()

# BUILT-IN
len()
```

Here's another example:
```python
x = 50
def func(x):
    print(f'X is {x}')
    
    # LOCAL REASSIGNMENT
    x = 200
    print(f'I just locally changed X to {x}')

func(x)
# X is 50
# I just locally changed X to 200

print(x)
# 50
```

When reassigning `x` argument in the local scope of `func` function, it doesn't affect the global scoped `x` variable.

Then what if we need to change the value in the global scope?
We can use the `global` keyword:
```python
x = 50
def func():
    global x
    print(f'X is {x}')
    
    # LOCAL REASSIGNMENT ON A GLOBAL VARIABLE
    x = 'GLOBALL'
    print(f'I just globally changed X to {x}')

func()
```
We are telling python "instead of accepting an argument, we want to use the `x` value in the global scope". So using `global` allows Python to reach out to that global and reassignment affects the global variable.

This could be dangerous as the code grows.
Let's refactor this code to something cleaner:
```python
x = 50
def func(x):
	print(f'X is {x}')

	x = 'RETURNING'
	print(f'I just locally changed X to {x}')
	return x

x = func(x)
# X is 50
# I just locally changed X to RETURNING

print(x)
# RETURNING
```
We just return the `x` by reassigning it locally and assign the returned value to the global `x`.


