
Python provides very helpful methods to read files.

## Writing to a file

Say we want to write a txt file in our working directory:
```python
%%writefile myfile.txt
Hello DK!
It is a pleasure meeting you.
```

## Reading from a file

Now I can open this file by storing in a variable:
```python
myfile = open('myfile.txt')
```
Note that if such file does not exist or is not in the same directory, it throws error, like `[Errno 2] No such file or directory: 'opps.txt'`.

Now we can read this file:
```python
myfile.read()
```
This will print: `'Hello DK!\nIt is a pleasure meeting you.\n'`.
Well, `\n` is just a line break.
Notice how if we run this command again, it prints out an empty string. This is because the cursor is now at the end of the text file. We need to tell it to be at the beginning:
```python
myfile.seek(0)
```

We can also get an array of text:
```python
myfile.readlines()
```
This will print: `['Hello DK!\n', 'It is a pleasure meeting you.\n']`.

Important to know that we need to ensure to close the file after opening a file, otherwise this will result an error.
```python
myfile.close()
```

There's a way to not worry about manually closing the file:
```python
with open('myfile.txt') as my_new_file:
	contents = my_new_file.read()
```
`my_new_file` is a variable of choice. After the colon, it creates an indentation so that any code there will use the `my_new_file` as a variable of the text file.
We now do not need to worry about closing the file. We can read the file by accessing `contents`:
```python
contents
```
`'Hello DK!\nIt is a pleasure meeting you.\n'`.

## Reading and writing to a file

Here are some modes to interact with a file:
- `mode='r'` - read only
- `mode='w'` - write only (will overwrite files or create new)
- `mode='a'` - append only (will add on to files)
- `mode='r+'` - reading and writing
- `mode='w+'` - writing and reading (overwrites existing files or creates a new file)

Let's first create a new txt file:
```python
%%writefile my_new_file.txt
ONE ON FIRIST
TWO ON SECOND
THREE ON THIRD
```

Let's read from the file:
```python
with open('my_new_file.txt', mode='r') as f:
	print(f.read())

# ONE ON FIRIST
# TWO ON SECOND
# THREE ON THIRD
```

We can append to the file:
```python
with open('my_new_file.txt', mode='a') as f:
    f.write('FOUR ON FOURTH')

# ONE ON FIRIST
# TWO ON SECOND
# THREE ON THIRD
# FOUR ON FOURTH
```

We can also write a new file(or overwrite an existing file):
```python
with open('qwe.txt', mode='w') as f:
    f.write('Hi! this is cool!')
```


