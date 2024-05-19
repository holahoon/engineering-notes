
1. **Use <code>for</code>, .split(), and <code>if</code> to create a Statement that will print out words that start with 's':**

```python
st = 'Print only the words that start with s in this sentence'
for item in st.split():
    if item[0].lower() == 's':
        print(item)
# start
# s
# sentence
```

2. **Use range() to print all the even numbers from 0 to 10.**
```python
nums = list(range(0, 11, 2))
# [0, 2, 4, 6, 8, 10]
```

3. **Use List comprehension to create a list of all numbers between 1 and 50 that are divisible by 3.**
```python
[x for x in range(1,51) if x % 3 == 0]
# [3, 6, 9, 12, 15, 18, 21, 24, 27, 30, 33, 36, 39, 42, 45, 48]
```

4. **Go through the string below and if the length of a word is even print "even!"**
```python
st = 'Print every word in this sentence that has an even number of letters'
for word in st.split():
    if len(word)%2 == 0:
        print(word + " <-- has an even length!")
# word <-- has an even length!
# in <-- has an even length!
# this <-- has an even length!
# sentence <-- has an even length!
# that <-- has an even length!
# an <-- has an even length!
# even <-- has an even length!
# number <-- has an even length!
# of <-- has an even length!
```

5. **Write a program that prints the integers from 1 to 100. But for multiples of three print "Fizz" instead of the number, and for the multiples of five print "Buzz". For numbers which are multiples of both three and five print "FizzBuzz".**
```python
for num in range(1,101):
    if num % 3 == 0 and num % 5 == 0:
        print("FizzBuzz")
    elif num % 3 == 0:
        print("Fizz")
    elif num % 5 == 0:
        print("Buzz")
    else:
        print(num)
```

6. **Use a List Comprehension to create a list of the first letters of every word in the string below:**
```python
st = 'Create a list of the first letters of every word in this string'
[word[0] for word in st.split()]
# ['C', 'a', 'l', 'o', 't', 'f', 'l', 'o', 'e', 'w', 'i', 't', 's']
```