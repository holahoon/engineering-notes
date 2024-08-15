
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
