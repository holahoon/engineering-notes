The type `[n]T` is an array of `n` values of type `T`.
The expression:
```go
var a [10]int
```
This declares a variable `a` as an array of ten integers.

An array's length is part of its type, so arrays cannot be resized. This seems limiting, but don't worry; Go provides a convenient way of working with arrays.

```go
func main(){
	var a[2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)
	// Hello World
	// [Hello World]
	

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
	// [2 3 5 7 11 13]
}
```
