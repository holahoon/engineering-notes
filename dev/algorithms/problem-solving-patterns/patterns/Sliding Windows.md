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

### Example 2.
Write a function called **minSubArrayLen** which accepts an array of integers and a number called **n**. The function should calculate the length of the array that is greater than or equal, and close to the **n**.
```js
minSubArrayLen([2,3,1,2,4,3], 7) // 2 -> because [4,3] is the smallest subarray
minSubArrayLen([2,1,6,5,4], 9) // 2 -> because [5,4] is the smallest subarray
minSubArrayLen([3,1,7,11,2,9,8,21,62,33,19], 52) // 1 -> because [62] is greater than 52
minSubArrayLen([1,4,16,22,5,7,8,9,10],39) // 3
minSubArrayLen([1,4,16,22,5,7,8,9,10],55) // 5
minSubArrayLen([4, 3, 3, 8, 1, 2, 3], 11) // 2
minSubArrayLen([1,4,16,22,5,7,8,9,10],95) // 0
```

#### Solution.
```js
function minSubArrayLen(arr, sum){
    let i = 0;
    let j = 0;
    let minLen = Infinity;
    let total = 0;
    
    while (i < arr.length) {
	    // If current window doesn't add up to the given sum, then move the window to right.
        if (total < sum && j < arr.length) {
            total += arr[j]
            j++
        } 
        // If current window adds up to at least the sum given, then we shrink the dinwo.
        else if (total >= sum) {
            minLen = Math.min(minLen, j - i);
            total -= arr[i]
            i++
        }
        // Current total is less than required total, but we reached the end. We need this, otherwise it'll be an infinite loop.
        else {
            break;
        }
    }
    
    return minLen === Infinity ? 0 : minLen
}
```

### Example 3.
Find the longest substring with unique characters in a given string.
```js
findLongestSubstring('') // 0
findLongestSubstring('rithmschool') // 7
findLongestSubstring('thisisawesome') // 6
findLongestSubstring('thecatinthehat') // 7
findLongestSubstring('bbbbbb') // 1
findLongestSubstring('longestsubstring') // 8
findLongestSubstring('thisishowwedoit') // 6
```

#### Solution.
```js
function findLongestSubstring(str){
	const seen = {};
	let start = 0;
	let maxLength = 0;

	for (let end = 0; end < str.length; end++){
		const char = str[end];
		// If the character is found in the map and is within the current window.
		if (seen[char] !== undefined && seen[char] >= start){
			// Move the start pointer to the right of the last occurrence.
			start = seen[char] + 1
		}
		// update the hash map with the current character's index.
		seen[char] = end;
		maxLength = Math.max(maxLength, end - start + 1);
	}
	return maxLength;
}
```
The time complexity for this solution is O(n).