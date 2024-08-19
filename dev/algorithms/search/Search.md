## Linear Search
- Check one element at a time to see if condition is met
### Linear Search Pseudocode
- This function accepts an array and a value
- Loop through the array and check if the current array element is equal to the value
- If it is, return the index at which the element is found
- If the value is never found, return -1
- 
```js
linearSearch([10, 15, 20, 25, 30], 15) // 1
linearSearch([9, 8, 7, 6, 5, 4, 3, 2, 1, 0], 4) // 5
```

```js
function linearSearch(arr, num){
	for (let i = 0; i < arr.length; i++){
		if (arr[i] === num) return i
	}
	return -1
}
```

This is pretty much the same function as `Array.indexOf`.

### The Big O.
The time complexity of this method is O(n).

## Binary Search
- Binary search is a much faster form of search
- Rather than eliminating one element at a time, you can eliminate *half* of the remaining elements at a time
- Binary search only works on *sorted* arrays!

### Binary Search Pseudocode
- This function accepts a sorted array and a value
- Create a left pointer at the start of the array, and a right pointer at the end of the array
- While the left pointer comes before the right pointer:
	- Create a pointer in the middle
	- If you find the value you want, return the index
	- If the value is too small, move the left pointer up
	- If the value is too large, move the right pointer down
- If you never find the value, return -1

#### Example.
Write a function which accepts a sorted array and a number, and find the index of the number if it exists, otherwise -1.
```js
binarySearch([1,2,3,4,5],3) // 2
binarySearch([1,2,3,4,5],5) // 4
binarySearch([1,2,3,4,5],6) // -1
binarySearch([5,6,10,13,14,18,30,34,35,37,40,44,64,79,84,86,95,96,98,99], 10) // 2
```

```js
function binarySearch(sortedArr, num){
    let left = 0;
    let right = sortedArr.length - 1
    let middle = Math.floor((left + right) / 2)
    
    while (left <= right) {
        if (sortedArr[middle] === num) return middle
        
        if (sortedArr[middle] > num) right = middle - 1
        else left = middle + 1
        
        middle = Math.floor((left + right) / 2)
    }
    return -1
}
```

### The Big O
Worst case and average case is O(log<sub>n</sub>). The best case is O(1).

## Naive String Search

### Pseudocode
- Loop over the longer string
- Loop over the shorter string
- If the characters don't match, break out of the inner loop
- If the characters do match, keep going
- If you complete the inner loop and find a match, increment the count of matches
- Return the count

```js
function naiveSearch(long, short){
	let found = 0
	for (let i = 0; i < long.length; i++){
		for (let j = 0; j < short.length; j++){
			if (long[i + j] !== short[j]) break
			if (j === short.lenght - 1) found++
		}
	}
	return found
}

naiveSearch('lorie loled', 'lol') // 1
```
