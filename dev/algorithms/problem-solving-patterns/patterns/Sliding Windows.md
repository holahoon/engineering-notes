This pattern involves creating a **window** which can either be any array or number from one position to another.
Depending on a certain condition, the window either increases or closes (and a new window is created).
Very useful for keeping track of a subset of data in an array/string etc.

### Example 1.
Write a function called **maxSubarraySum** which accepts an array of integers and a number called **n**. The function should calculate the maximum sum of **n** consecutive elements in the array.
```js
maxSubarraySum([1,2,5,2,8,1,5], 2) // 10
maxSubarraySum([1,2,5,2,8,1,5], 4) // 17
maxSubarraySum([4,2,1,6], 1) // 6
maxSubarraySum([4,2,1,6,2,], 4) // 13
maxSubarraySum([], 4) // null
```

#### Solution.
```js
function maxSubarraySum(arr, num) {
	if (arr.length < num) return null
	let maxSum = 0
	let tempSum = 0
	// Intialize the maxSum to be the max value
	for (let i = 0; i < num; i++){
		maxSum += arr[i]
	}

	// give tempSum the current maxSum value
	tempSum = maxSum

	// For each iteration with num window, compare the total value of the current window with the maxSum
	for (let i = num; i < arr.length; i++){
		tempSum = tempSum - arr[i - num] + arr[i]
		maxSum = Math.max(tempSum, maxSum)
	}
	return maxSum
}
```
We first get the total sum of the window up to `num`. Say `num` is 3, we sum 0, 1, 2 indexed elements in the array. We then store that total sum as `maxSum` and give `tempSum` the current `maxSum` value. We then iterate over the array again, but now we start from `num` index. This would make the loop start at index 3. We subtract the first element from the window which is the element at index 0 and add the element at index `num` (in our case, index 3). This goes on by sliding the window by 3, subtracting the first element of the window and adding in the element after the window.

