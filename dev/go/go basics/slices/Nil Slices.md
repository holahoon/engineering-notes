The zero value of a slice is `nil`.
A nil slice has a length and capacity of 0 and has no underlying array.
```go
func main(){
	var s []int
	fmt.Println(s, len(s), cap(s))
	// [] 0 0

	if s == nil {
		fmt.Println("nil!")
		// nil!
	}
}
```
