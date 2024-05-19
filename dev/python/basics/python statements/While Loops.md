```python
x = 0

while x < 5:
    print(f'The current value of x is {x}')
    x += 1
else:
    print('X is not less than 5')
```
Pretty cool how you can use an else in a while loop.

## Pass, Break, Continue

### Pass

```python
x = [1, 2, 3]
for item in x:
    # comment
    pass
print('end of loop')
```
Python requires indentations to execute the next code block. If we just have the `# comment` in a place where an executable code is needed, Python will throw an error. There, we can just use `pass`.

### Continue

`continue` basically just skips the execution:
```python
mystring = 'David'

for letter in mystring:
    if letter == 'a':
        continue
    print(letter)

# D
# v
# i
# d
```

### Break

`break` basically breaks out of the execution:
```python
mystring = 'David'

for letter in mystring:
    if letter == 'a':
        break
    print(letter)

# D
```

We can also use this `break` in a while loop:
```python
x = 0
while x < 5:
    if x == 3:
        break
    print(x)
    x += 1
```


