This pattern uses objects or sets to collect values/frequencies of values.
This can often avoid the need for nested loops or O(n<sup>2</sup>) operations with arrays/strings.

### Example 1.
Write a function called **same**, which accepts two arrays. The function should return true if every value in the array has its corresponding value squared in the second array. The frequency of values must be the same.
```js
same([1,2,3], [4,1,9]) // true
same([1,2,3], [1,9]) // false
same([1,2,1], [4,4,1]) // false (must be same frequency)
```

#### A naive solution:
```js
function same(arr1, arr2){
	if (arr1.length !== arr2.length) return false
	for (i = 0; i < arr1.length; i++){
		let correctIndex = arr2.indexOf(arr1[i] ** 2)
		if (correctIndex === -1) return false;
		arr2.splice(correctIndex, 1)
	}
	return true
}
```
Time complexity for this solution is O(n<sup>2</sup>).

#### A refactored solution:
```js
function same(arr1, arr2) {
	if (arr1.length !== arr2.length) return false
	let frequencyCounter1 = {}
	let frequencyCounter2 = {}
	for (const val of arr1) {
		frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1
	}
	for (const val of arr2) {
		frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1
	}
	for (const key in frequencyCounter1){
		if (!(key ** 2 in frequencyCounter2)) return false
		if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) return false
	}
	return true
}
```
Time complexity for this solution if O(n).

### Example 2: Anagram.
Given two strings, write a function to determine if the second string is an anagram of the first. An anagram is a word, phrase, or name formed by rearranging the letters of another, such as *cinema*, formed from *iceman*.
```js
validAnagram('', '') // true
validAnagram('aaz', 'zza') // false
validAnagram('anagram', 'nagaram') // true
validAnagram('rat', 'car') // false
validAnagram('awesome', 'awesom') // false
validAnagram('texttwisttime', 'timetwisttext') // true
```

#### Solution 1.
```js
function validAnagram(str1, str2){
    // return if the length of both strings are not equal.
    if (str1.length !== str2.length) return false
    
    // create sets
    let freq1 = {}
    let freq2 = {}
    
    // iterate over and store the frequency of each letter.
    for (const str of str1) {
        freq1[str] = (freq1[str] || 0) + 1
    }
    for (const str of str2) {
        freq2[str] = (freq2[str] || 0) + 1
    }
    
    for (const key in freq1) {
        if (!key in freq2) return false
        if (freq1[key] !== freq2[key]) return false
    }
    return true
}
```
Time complexity for this solution if O(n).

#### Solution 2.
```js
function validAnagram(str1, str2){
    if (str1.length !== str2.length) return false
    
    const lookup = {}
    
    for (const str of str1){
	    lookup[str] = (lookup[str] || 0) + 1
    }
    for (const str of str2){
	    if (!lookup[str]) return false
	    lookup[str] -= 1
    }
    return true
}
```
Time complexity for this solution if O(n).