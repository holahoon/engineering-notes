An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

The type `[]T` is a slice with elements of type `T`.

A slice is formed by specifying two indices, a low and high bound, separated by a colon:
```go
a[low: high]
```
This selects a half-open range which includes the first element, but excludes the last one.

The following expression creates a slice which includes elements 1 through 3 of `a`:
```go
a[1:4]
```

Example:
```go
func main(){
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	var k []int = primes[3:5]
	fmt.Println(s) // [3 5 7]
	fmt.Println(k) // [7 11]
}
```

## Slices are like references to arrays

A slice does not store any data, it just describes a section of an underlying array.
Changing the elements of a slice modifies the corresponding elements of its underlying array.
Other slices that share the same underlying array will see those changes.

```go
func main(){
	names := [4]string{
		"John",
		"David",
		"George",
		"Chris"
	}
	fmt.Println(names)
	// [John David George Chris]

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)
	// [John David] [David George]

	b[0] = "XXX"
	fmt.Println(a, b)
	// [John XXX] [XXX George]
	fmt.Println(names)
	// [John XXX George Chris]
}
```
Notice how changing the 0th index of `b` to `"XXX"` changes the underlying array - `names`'s `"David"` value to `"XXX"`.
Slices are like references to arrays and it can modify the original array.

## Slice literals

A slice literal is like an array literal without the length.
This is an array literal:
```go
[3]bool{true, true, true}
```

And this creates the same arrays as above, then builds a slice that references it:
```go
[]bool{true, true, false}
```

Example:
```go
func main(){
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)
	// [2 3 5 7 11 13]

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)
	// [true, false, true, true, false, true]

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
	// [{2 true} {3 false} {5 true} {7 true} {11 false} {13 true}]
}
```

## Slice defaults

when slicing, you may omit the high or low bounds to use their defaults instead.
The default is zero for the low bound and the length of the slice for the high bound.
For the array
```go
var a [10]int
```
these slices expressions are equivalent:
```go
a[0:10]
a[:10]
a[0:]
a[:]
```

Example:
```go
func main(){
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)
	// [3 5 7]

	s = s[:2]
	fmt.Println(s)
	// [3 5]

	s = s[1:]
	fmt.Println(s)
	// [5]
}
```

## Slice length and capacity

A slice has both a *length* and a *capacity*.
- The length of a slice is the number of elements it contains.
- The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.
The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`.
You can extend a slice's length by re-slicing it, provided it has sufficient capacity.
```go
func main(){
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)
	// len=6 cap=6 [2 3 5 7 11 13]

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)
	// len=0 cap=6 []

	// Extend its length.
	s = s[:4]
	printSlice(s)
	// len=4 cap=6 [2 3 5 7]

	// Drop its first two elements.
	s = s[2:]
	printSlice(s)
	// len=2 cap=4 [5 7]
}

func printSlice(s []int){
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

Notice how when slicing the slice to give it zero length doesn't change the capacity of the slice, but when dropping the first two elements changes the capacity of `s`.
Asked ChatGPT why this is and it says:
When you use `s = s[:0]`, you're not moving the start of the slice. You're only adjusting its length to zero while retaining the original starting index. Since you haven't changed the starting position, `s` still has the capacity from the starting index to the end of the underlying array, which is 6.
In Go, the capacity of a slice is calculated from its starting index to the end of the underlying array. Because `s[:0]` doesn't change the starting position, the capacity remains as it was in the original slice.
When you do `s = s[2:]`, you change the starting position of the slice to index `2` of the underlying array. Now, the slice `s` starts from the 2nd element of the original array (the element `5`), which means the length of the remaining portion of the array has decreased.
Now, `s` has a capacity of `4` because it can only "see" four elements (`[5, 7, 11, 13]`) from its new starting position (index 2) to the end of the original array.
So, basically:
- `s[:0]` adjusts only the length to zero, keeping the starting position, so the capacity remains the same.
- `s[2:]` shifts the starting position of the slice to index `2`, reducing the capacity to the remaining elements from that point onward.
