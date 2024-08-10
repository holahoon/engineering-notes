Creating **pointers** or values that correspond to an index or position and move towards the beginning, end or middle based on a certain condition.
**Very** efficient for solving problems with minimal space complexity as well.

### Example 1.
Write a function called **sumZero** which accepts a **sorted** array of integers. The function should find the **first** pair where the sum is 0. Return an array that includes both values that sum to zero or undefined if a pair does not exist.
```js
sumZero([-3,-2,-1,0,1,2,3]) // [-3,3]
sumZero([-2,0,1,3]) // undefined
sumZero([1,2,3]) // undefined
```

#### Naive solution
```js
function sumZero(arr) {
	for (let i = 0; i < arr.length; i++) {
		for (let j = i+1; j < arr.lenth){
			if (arr[i] + arr[j] === 0) return [arr[i], arr[j]]
		}
	}
}
```

#### Refactored solution.
```js
function sumZero(arr) {
	let left = 0
	let right = arr.length - 1

	while (left < right){
		const sum = arr[left] + arr[right]
		if (sum === 0) return [arr[left], arr[right]]
		if (sum > 0) right--
		else left++
	}
}
```

### Example 2.
Implement a function called **counterUniqueValues**, which accepts a sorted array, and counts the unique values in the array. There can be negative numbers in the array, but it will always be sorted.
```js
countUniqueValues([1,1,1,1,1,2]) // 2
countUniqueValues([1,2,3,4,4,4,7,7,12,12,13]) // 7
countUniqueValues([]) // 0
countUniqueValues(-2,-1,-1,0,1) // 4
```

Since there's no restriction about mutating the array, we can use a two-pointers.
```js
function countUniqueValues(arr){
	if (!arr.length) return 0
	let i = 0
	let j = 1
	while(j < arr.length){
		if (arr[i] !== arr[j]) {
			i++
			arr[i] = arr[j]
			j++
		} else {
			j++
		}
	}
	return i + 1
}
```
We basically make `i` to start incrementing if the element at `i`th position and the element at `j`th position are not same.
We can also use a for loop.
```js
function counterUniqueValues(arr){
	if (!arr.length) return 0
	let i = 0
	for (let j = 0; j < arr.length; j++){
		if (arr[i] !== arr[j]){
			i++
			arr[i] = arr[j]
		}
	}
}
```
