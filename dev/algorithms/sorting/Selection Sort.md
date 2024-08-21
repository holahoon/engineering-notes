Similar to [[Bubble Sort]], but instead of first placing large values into sorted position, it places small values into sorted position.

### Selection Sort Pseudocode
- Store the first element as the smallest value you've seen so far
- Compare this item to the next item in the array until you find a smaller number
- If a smaller number is found, designate that smaller number to be the new "minimum" and continue until the end of the array
- If the "minimum" is not the value(index) you initially began with, swap the two values
- Repeat this with the next element until the array is sorted

```js
function swap(arr, idx1, idx){
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]]
}

function selectionSort(arr){
	let lowest = 0

	for (let i = 0; i < arr.length; i++){
		for (j = i + 1; j < arr.length; j++){
			if (arr[j] < arr[i]) lowest = j
		}
		if (i !== lowest) swap(arr, i, lowest)
	}
	return arr
}

selectionSort([34, 22, 10, 19, 17])
```

### The Time Complexity
O(n<sup>2</sup>).
Selection sort is better than bubble sort when you need to make less swaps.