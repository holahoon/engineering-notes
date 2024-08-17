
Just a classic example of using recursion in solving a factorial problem:

### Example 1.
```js
function factorial(num){
	if (num === 1) return 1
	return num * factorial(num - 1)
}
```

## Helper Method Recursion

```js
function outer(input){
	const outerScopedVariable = [];

	function helper(helperInput){
		// modify the outerScopedVariable
		helper(helperInput--)
	}
	helper(input)

	return outerScopedVariable;
}
```

### Example.
Let's try to collect all of the odd values in an array!
```js
function collectOddValues(arr){
	const result = [];

	function helper(helperInput){
		if (helperInput.length === 0) return;
		if (helperInput[0] % 2 !== 0) result.push(helperInput[0]);
		helper(helperInput.slice(1));
	}
	helper(arr);
	
	return result;
}
```

## Pure Recursion

We can take the same problem, but in a pure recursion way.
```js
function collectOddValues(arr){
	let newArr = [];
	if (!arr.length) return newArr;
	if (arr[0] % 2 !== 0) newArr.push(arr[0]);

	newArr = newArr.concat(collectedOddValues(arr.slice(1)));
	return newArr;
}
```

#### Tips
For arrays, use methods like **slice**, **the spread operator**, and **concat** that make copies of arrays so you do not mutate them.

### Practice problems:

#### Example 1.
Write a function that works like `Math.pow()`.
```js
power(2,0) // 1
power(2,2) // 4
power(2,4) // 16
```

```js
function power(base, exponent){
    if (exponent === 0) return 1
    return base * power(base, exponent - 1)
}
```

#### Example 2.
Write a factorial function.
```js
factorial(1) // 1
factorial(2) // 2
factorial(4) // 24
factorial(7) // 5040
```

```js
function factorial(num){
    if (num === 0) return 1
    return num * factorial(num - 1)
}
```

#### Example 3.
Write a function which accepts an array of numbers and multiplies all the numbers.
```js
productOfArray([1,2,3]) // 6
productOfArray([1,2,3,10]) // 60
```

```js
function productOfArray(arr){
    let num = arr[0]
    if (!arr.length) return 1
    return num *= productOfArray(arr.slice(1))
}
```

#### Example 4.
Write a function which accepts a number that adds up all the numbers from 0.
```js
recursiveRange(6) // 21
recursiveRange(19) // 55
```

```js
function recursiveRange(num){
    if (num === 0) return 0
    return num + recursiveRange(num - 1)
}
```

#### Example 5.
Write a fibonacci function where it returns the number at nth place. Each number is the sum of the two preceding ones.
```js
fib(4) // 3
fib(10) // 55
fib(28) // 317811
fib(45) // 9227465
```

```js
function fib(num){
	if (num <= 2) return 1
	return fib(n - 1) + fib(n - 2)
}
```

#### Example 6.
Write a function that accepts a string and reverses it and returns.
```js
reverse('awesome') // 'emosewa'
reverse('rithmschool') // 'loohcsmhtir'
```

```js
function reverse(word){
  if (!word.length) return ""
  return reverse(word.slice(1)) + word[0]
}
```

#### Example 7.
Write a function which check if a word is palindrome. Reading from the beginning or the end must be same to return true, otherwise false.
```js
isPalindrome('awesome') // false
isPalindrome('foobar') // false
isPalindrome('tacocat') // true
isPalindrome('amanaplanacanalpanama') // true
isPalindrome('amanaplanacanalpandemonium') // false
```

```js
function isPalindrome(word){
  if (word.length < 2) return true
  if (word[0] === word[word.length - 1]) return isPalindrome(word.slice(1, -1))
  return false
}
```

#### Example 8.
Write a recursive function which accepts an array and a callback method. This function should return true if the callback method returns true, otherwise false.
```js
// SAMPLE INPUT / OUTPUT
const isOdd = val => val % 2 !== 0;

someRecursive([1,2,3,4], isOdd) // true
someRecursive([4,6,8,9], isOdd) // true
someRecursive([4,6,8], isOdd) // false
someRecursive([4,6,8], val => val > 10); // false
```

```js
function someRecursive(arr, cb){
	if (!arr.length) return false
	if (cb(arr[0])) return true
	return someRecursive(arr.slice(1), cb)
}
```

#### Example 9.
Write a recursive function that accepts nested arrays and returns by flattening the nested arrays.
```js
flatten([1, 2, 3, [4, 5] ]) // [1, 2, 3, 4, 5]
flatten([1, [2, [3, 4], [[5]]]]) // [1, 2, 3, 4, 5]
flatten([[1],[2],[3]]) // [1,2,3]
flatten([[[[1], [[[2]]], [[[[[[[3]]]]]]]]]]) // [1,2,3]
```

Solution:
```js
function flatten(arrays){
  let newArr = []
  for (const el of arrays) {
      if (Array.isArray(el)) newArr = newArr.concat(flatten(el))
      else newArr.push(el)
  }
  return newArr
}
```

#### Example 10.
Write a recursive function which accepts an array of strings and capitalizes each word.
```js
capitalizeFirst(['car','taco','banana']); // ['Car','Taco','Banana']
```

Solution:
```js
function capitalizeFirst (words) {
    if (!words.length) return []
    const capitalizedWord = words[0][0].toUpperCase() + words[0].slice(1)
    return [capitalizedWord].concat(capitalizeFirst(words.slice(1)))
}
```

#### Example 11.
Write a recursive function that adds up all the even numbers in a nested object.
```js
const obj1 = {
  outer: 2,
  obj: {
    inner: 2,
    otherObj: {
      superInner: 2,
      notANumber: true,
      alsoNotANumber: "yup"
    }
  }
} // 6
const obj2 = {
  a: 2,
  b: {b: 2, bb: {b: 3, bb: {b: 2}}},
  c: {c: {c: 2}, cc: 'ball', ccc: 5},
  d: 1,
  e: {e: {e: 2}, ee: 'car'}
} // 10
```

Solution 1:
```js
function nestedEvenSum(obj){
	let sum = 0
	for (const key in obj){
		// null in JS is also of type object btw.
		if (typeof obj[key] === 'object' && obj[key] !== null){
			sum += nestedEvenSum(obj[key])
		} else {
			if (obj[key] % 2 === 0) sum += obj[key]
		}
	}
	return sum
}
```

Solution 2:
```js
function nestedEvenSum(obj, sum = 0){
	for (const key in obj){
		const el = obj[key]
		if (typeof el === 'object') sum += nestedEvenSum(el)
		else if (typeof el === 'number' && el % 2 === 0) sum += el
	}
	return sum
}
```

#### Example 12.
Write a recursive function which accepts a nested object and transforms a value that is of type number to a string type.
```js
const obj = {
    num: 1,
    test: [],
    data: {
        val: 4,
        info: {
            isRight: true,
            random: 66
        }
    }
}
stringifyNumbers(obj)
// converted:
{
    num: "1",
    test: [],
    data: {
        val: "4",
        info: {
            isRight: true,
            random: "66"
        }
    }
}
```

Solution 1:
```js
function stringifyNumbers(obj, base = {}){
	for (const key in obj){
		const el = obj[key]
		if (typeof el === 'object' && !Array.isArray(el) && el !== null){
			base[key] = stringifyNumbers(el)
		} else if (typeof el === 'number') {
			base[key] = `${el}`
		} else {
			base[key] = el
		}
	}
	return base
}
```
#### Example 13.
Write a recursive function that accepts a nested object and takes the values that are of type string, and returns them as an array.
```js
const obj = {
    stuff: "foo",
    data: {
        val: {
            thing: {
                info: "bar",
                moreInfo: {
                    evenMoreInfo: {
                        weMadeIt: "baz"
                    }
                }
            }
        }
    }
}
collectStrings(obj) // ["foo", "bar", "baz"])
```

solution:
```js
function collectStrings(obj, base = []){
    for (const key in obj){
        const el = obj[key]
        
        if (typeof el === 'object' && el !== null && !Array.isArray(el)){
            return base.concat(collectStrings(el))
        } else if (typeof el === 'string'){
            base.push(el)
        }
    }
    return base
}
```

Solution 2 (helper method recursive version):
```js
function collectStrings(obj){
	let arr = []

	function gatherStrings(o){
		for (const key in o){
			if (typeof o[key] === 'string') arr.push(o[key])
			else if (typeof o[key] === 'object') return gatherStrings(o[key])
		}
	}
	gatherStrings(obj)
	return arr
}
```
