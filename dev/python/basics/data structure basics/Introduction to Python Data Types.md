- Integers (int) - Whole numbers, such as: `3`, `300`, `200`
- Floating point (float) - Numbers with a decimal point: `2.3`, `4.6`, `100.0`
- Strings (str) - Ordered sequence of characters: `"hello"`, `"Sammy"`, `"2000"`, `"후니"`
- Lists (list) - Ordered sequence of objects: `[10, "hello", 200.3]`
- Dictionaries (dict) - Unordered Key:Value pairs: `{"myKey": "value", "name": "DK"}`
- Tuples (tup) - Ordered immutable sequence of objects: `(10, "hello", 200.3)`
- Sets (set) - Unordered collection of unique objects: `{"a", "b"}`
- Booleans (bool) - Logical value indicating `True` of `False`

## Strings
Here's a pretty cool Python way of reversing a string using slicing!
```python
s = 'hello'
s[::-1] # 'olleh'
```
`[start:end:step]`

## Lists
Lists are ordered sequences that can hold a variety of object types. they use `[]` brackets and commas to separate objects in the list. `[1, 2, 3, 4]`. Lists support indexing and slicing. Lists can be nested and also have a variety of useful methods that can be called off of them.

Here's are couple ways of creating a list:
```python
# method 1.
[0] * 3 # [0, 0, 0]

# method
list2 = [0, 0, 0]
```

## Dictionaries
Dictionaries are unordered mappings for storing objects. Dictionaries use a key-value pairing. This key-value pari allows users to quickly grab objects without needing to know an index location.

## Tuples
Tuples are very similar to lists, however they are **immutable**. Once an element is inside a tuple, it cannot be reassigned. Tuples use parenthesis: `(1, 2, 3)`.

## Sets
Sets are unordered collections of unique elements, meaning there can only be one representative of the same object. Sets don't have any particular order.
example:
```python
myset = set()
myset.add(1)
# 1
```
