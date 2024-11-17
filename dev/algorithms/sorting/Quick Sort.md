- Like [[Merge Sort]], exploits the fact that arrays of 0 or 1 element are always sorted.
- Works by selecting one element (called the "pivot") and finding the index where the pivot should end up in the sorted array
- Once the pivot is positioned appropriately, quick sort can be applied on either side of the pivot

### Pivot Helper
- In order to implement merge sort, it's useful to first implement a function responsible arranging elements in an array on either side of a pivot.
- Given an array, this helper function should designate an element as the pivot.
- It should then rearrange elements in the array so that all values less than the pivot are moved to the left of the pivot, and all values greater than the pivot are moved to the right of the pivot.
- The order of elements on either side of the pivot doesn't matter!
- The helper should do this **in place**, that is, it should not create a new array
- When complete, the helper should return the index of the pivot

### Picking a Pivot
- The runtime of quick sort depends in part on how one selects the pivot.
- Ideally, the pivot should be chosen so that it's roughly the median value in the data set you're sorting.
- For simplicity, we'll always choose the pivot to be the first element.

### PIvot Helper Example
```javascript
let arr = [5, 2, 1, 8, 4, 7, 6, 3]

pivot(arr) // 4 <- index of `5`.

arr;
// [2, 1, 4, 3, 5, 8, 7, 6] <- `5` is placed on 4th index.
// The order of the elements on the left side does not matter
```

### Pivot Pseudocode
- It will help to accept three arguments; an array, a start index, and an end index (these can default to 0 and the array length minus 1, respectively).
- Grab the pivot from the start of the array
- Store the current pivot index in a variable (this will keep track of where the pivot should end up)
- Loop through the array from the start until the end
	- If the pivot is greater than the current element, increment the pivot index variable and then swap the current element with the element at the pivot index
- Swap the starting element (i.e. the pivot) with the pivot index
- Return the pivot index

```javascript
function swap(arr, i, j) {
	[arr[i], arr[j]] = [arr[j], arr[i]]
}

function pivotHelper(arr, start = 0, end = arr.length + 1) {
	let pivot = arr[start];
	let swapIndex = start;

	for (let i = start + 1; i < arr.length; i++) {
		if (pivot > arr[i]) {
			swapIndex++;
			swap(arr, swapIndex, i);
		}
	}
	swap(arr, start, swapIndex);
	return swapIndex;
}

const EXAMPLE = [4, 8, 2, 1, 5, 7, 6, 3];

pivot(EXAMPLE) // 3
// [4, 8, 2, 1, 5, 7, 6, 3] swapIndex = 0
// [4, 2, 8, 1, 5, 7, 6, 3] swapIndex = 1
// [4, 2, 1, 8, 5, 7, 6, 3] swapIndex = 2
// ...
// [4, 2, 1, 3, 8, 5, 7, 6] swapIndex = 3
// [2, 1, 3, 4, 8, 5, 7, 6] swapped and swapIndex = 3
```

## Quick Sort Pseudocode
- Call the pivot helper on the array.
- When the helper returns to you the updated pivot index, recursively call the pivot helper on the subarray to the left of that index, and the subarray to the right of that index.
- Your base case occurs when you consider a subarray with less than 2 elements.

```javascript
quickSort([4,6,9,1,2,5,3])
// [4,6,9,1,2,5,3]
// [3,2,1,4,6,9,5]
//        4
//  3,2,1   6,9,5
//      3     6
//  2,1     5   9
//    2
//  1
```

### Code
```javascript
// ...swap function
// ...pivot function

function quickSort(arr, left = 0, right = arr.length - 1) {
	if (left < right) {
		let pivotIndex = pivot(arr, left, right);

		// Left side of the pivotIndex
		quickSort(arr, left, pivotIndex - 1);

		// Right side of the pivotIndex
		quickSort(arr, pivotIndex + 1, right);
	}

	return arr;
}
```
## Big O

#### Time Complexity
- Best - O(n log n)
- Average- O(n log n)
- Worst O(n<sup>2</sup>)

#### Space Complexity
- O(log n)