A sorting algorithm where the largest values bubble up to the top.
Many sorting algorithms involve sone type of swapping functionality (e.g. swapping to numbers to put them in order).
```js
// ES5
function swap(arr, idx1, idx2){
	const temp = arr[idx];
	arr[idx1] = arr[idx2];
	arr[idx2] = temp;
}

// ES2015
const swap = (arr, idx1, idx2) => {
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
}
```

### Bubble Sort Pseudocode
- Start looping from the end of the array with a variable called *i* towards the beginning
- Start an inner loop with a variable called *j* from the beginning until `i - 1`.
- If `arr[j]` is greater than `arr[j + 1]`, swap those two values!
- Return the sorted array

#### Naive solution.
```js
function bubbleSort(arr){
	for (let i = arr.length; i > 0; i--){
		for (let j = 0; j < i - 1; j++){
			if (arr[j] > arr[j + 1]){
				[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]
			}
		}
	}
	return arr
}
bubbleSort([37,45,29,8])
```

#### Optimized solution.
```js
function swap(arr, idx1, idx2){
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]]
}

function bubbleSort(arr){
	let isSwap;
	for (let i = arr.length; i > 0; i--){
		for (let j = 0; j < i - 0; j++){
			if (arr[j] > arr[j + 1]) {
				swap(arr, j, j + 1)
				isSwap = false
			}
		}
	if (isSwap) break
	}
}
bubbleSort([8,1,2,3,4,5,6,7])
```
We set a `isSwap` flag to see if there's anything to swap. If the inner loop iterates over the remaining array until wherever the *i* is and if the inner loop doesn't swap at all, that means there's nothing else to swap, so we break out of the outer loop early.
This is efficient if the array is nearly sorted like the example.

### Time complexity
In general, it's O(n<sup>2</sup>). However, if the data is nearly sorted, it can be simplified down to O(n).
But, that's just the best case. It's O(n<sup>2</sup>).
