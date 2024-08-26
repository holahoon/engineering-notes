In Go, arrays have a *fixed capacity* which you define when you declare the variable. We can initialize an array in two ways:
- [N]type{value1, value2, ...valueN} e.g. `numbers := [5]int{1, 2, 3, 4, 5}`
- [...]type{value1, value2, ...valueN} e.g. `numbers := [...]int{1, 2, 3, 4, 5}`

It is sometimes useful to also print the inputs to the function in the error message. Here, we use the `%v` placeholder to print the "default" format: `"Got %v"`.

### Using `range` to help with array iteration

We can use `range`:
```go
func Sum(numbers [5]int) int {
	sum := 0
	for _, number := range numbers {
		sum += number
	}
	return sum
}
```

`range` lets you iterate over an array. On each iteration, `range` returns two values - the index and the value. We are ignoring the index value by using `_` [blank identifier](https://go.dev/doc/effective_go#blank).

### Arrays and their type

An interesting property of arrays is that the size is encoded in its type. If you try to pass an `[4]int` into a function that expects `[5]int`, it won't compile. They are different types so it's just the same as trying to pass a `string` into a function that wants an `int`.

You may be thinking it's quite cumbersome that arrays have a fixed length, and most of the time you probably won't be using them!

Go has *slices* which do not encode the size of the collection and instead can have any size.

## Using slices

We can take the existing `Sum` function which expects a fixed array to expect it to receive a slice.
```go
func Sum(numbers []int) int {
	...
}
```

### Example
Say we want to add all the sum of n number of arrays of integers.
```go
// test function
func TestSumAll(t *testing.T){
	got := sumAll([]int{1,2}, []int{0,9})
	want := []int{3,9}

	if (reflect.DeepEqual(got, want)){
		t.Errorf("Got %v, want %v", got, want)
	}
}
```

```go
func SumAll(numbersToSum ...[]int) []int {
	lengthOfNumbers := len(numbersToSum)
	sums := make([]int, lengthOfNumbers)

	for i, numbers := range numbersToSum {
		sums[i] = Sum(numbers)
	}
	return sums
}
```

There's a new way to create a slice. `make` allows you to create a slice with a starting capacity of the `len` of the `numbersToSum` we need to work through. The length of a slice is the number of elements it holds `len(mySlice)`, while the capacity is the number of elements it can hold in the underlying array `cap(mySlice)`, e.g., `make([]int, 0, 5)` creates a slice with length 0 and capacity of 5.
You can index slices like arrays with `mySlice[N]` to get the value out or assign it a new value with `=`.

#### Let's refactor this code.
As mentioned, slices have a capacity. If you have a slice with a capacity of 2 and trying to do `mySlice[10] = 1`, you will get a *runtime error*.
However, you can use the `append` function which takes a slice and a new value, then returns a new slice with all the items in it.
```go
func SumAll(numbersToSum ...[]int) []int {
	var sums []int
	for _, numbers := range numbersToSum {
		sums = append(sums, Sum(numbers))
	}
	return sums
}
```
With this implementation, we don't need to worry about capacity. We start with an empty slice `sums` and append to it the result of `Sum` as we work through the varargs.

