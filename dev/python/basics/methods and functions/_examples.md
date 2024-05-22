### OLD MACDONALD: Write a function that capitalizes the first and fourth letters of a name
```
old_macdonald('macdonald') --> MacDonald
```
Note: `'macdonald'.capitalize()` returns `'Macdonald'`.

```python
def old_mcdonald(name):
	return name[:3].capitalize() + name[3:].capitalize()

old_macdonald('macdonald') # 'MacDonald'
```

### MASTER YODA: Given a sentence, return a sentence with the words reversed
```
master_yoda('I am home') --> 'home am I'
master_yoda('We are ready') --> 'ready are We'
```

```python
def master_yoda(text):
	letters = text.split()
	return ' '.join(letters[::-1])

master_yoda('I am home') # 'home am I'
```

### ALMOST THERE: Given an integer n, return True if n is within 10 of either 100 or 200
```
almost_there(90) --> True
almost_there(104) --> True
almost_there(150) --> False
almost_there(209) --> True
```

```python
def almost_there(n):
	return abs(100 - n) <= 10 or abs(200 - n) <= 10
```

### Given a list of ints, return True if the array contains a 3 next to a 3 somewhere.
```
has_33([1, 3, 3]) → True
has_33([1, 3, 1, 3]) → False
has_33([3, 1, 3]) → False
```

```python
def has_33(nums):
    for i in range(0, len(nums) - 1):
        if nums[i] == 3 and nums[i + 1] == 3:
            return True
    return False
```

### PAPER DOLL: Given a string, return a string where for every character in the original there are three characters
```
paper_doll('Hello') --> 'HHHeeellllllooo'
paper_doll('Mississippi') --> 'MMMiiissssssiiippppppiii'
```

```python
def paper_doll(text):
    result = ''
    for letter in text:
        result += letter * 3
    return result
```

### BLACKJACK: Given three integers between 1 and 11, if their sum is less than or equal to 21, return their sum. If their sum exceeds 21 *and* there's an eleven, reduce the total sum by 10. Finally, if the sum (even after adjustment) exceeds 21, return 'BUST'
```
blackjack(5,6,7) --> 18
blackjack(9,9,9) --> 'BUST'
blackjack(9,9,11) --> 19
```

```python

```


