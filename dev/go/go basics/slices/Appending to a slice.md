It is common to append new elements to a slice, and so Go provides a built-in `append` function. The [documentation](https://pkg.go.dev/builtin#append) of the built-in package describes `append`.
```go
func append(s []T, vs ...T) []T
```

The first parameter `s` of `append` is a slice of type `T`, and the rest are `T` values to append to the slice.
The resulting value of `append` is a slice containing all the elements of the original slice plus the provided values.
If the backing array of `s` is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

```go
func main(){
	var s []int
	printSlice(s)
	// len=0 cap=0 []

	// The append works on nil slices.
	s = append(s, 0)
	printSlices(s)
	// len=1 cap=1 [0]

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s)
	// len=2 cap=2 [0 1]

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s)
	// len=5 cap=6 [0 1 2 3 4]
}

func printSlice(s []int){
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

Notice how the last `cap=6`.
This is due to how Go dynamically allocates memory when appending elements to a slice. Here's a break down of each `append`:

1. Starting slice: `var s []int`
	- This creates a `nil` slice with length and capacity both set to 0.
	- Output: `len=0 cap=0 []`
2. First append -Appending one element: `s = append(s, 0)`
	- Since `s` is currently `nil` (with capacity of 0), Go allocates space for the slice and sets an initial capacity, which is typically 1.
	- Output: `len=1 cap=1 [0]`
3. Second append - Appending another element: `s = append(s, 1)`
	- The slice's current capacity (1) is full, so Go double the capacity to make room for more elements.
	- The capacity is increased to 2.
	- Output: `len=2 cap=2 [0 1]`
4. Third append - Appending multiple elements: `s = append(s, 2, 3, 4)`
	- The current capacity of `s` is 2, but you're trying to add three more elements.
	- To accommodate these elements, Go again resizes the capacity. Generally, Go's append mechanism doubles the capacity when resizing. So the capacity doubles from 2 to 4, but this is still insufficient for all elements.
	- As a result, Go increases the capacity to 6 (enough to hold the give elements) and copies the existing elements to a new array of this larger capacity.
	- Output `len=5 cap=6 [0 1 2 3 4]`
So basically, the capacity is 6 because Go doubled the capacity as it appended more elements, growing dynamically to handle the additional elements. Go's strategy to expand capacity generally involves doubling the previous capacity, but it sometimes varies slightly to reduce allocations, which can lead to capacities like 6 instead of a strict doubling.
