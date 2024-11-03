The `range` form of the `for` loop iterates over a slice or map.
When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.
```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main(){
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}

// 2**0 = 1
// 2**1 = 2
// 2**2 = 4
// 2**3 = 8
// 2**4 = 16
// 2**5 = 32
// 2**6 = 64
// 2**7 = 128
```

You can skip the index or value by assigning to `_`.
```go
for i, _ := range pow
for _, value := range pow
```

If you only want the index, you can omit the second variable.
```go
for i := range pow
```

Example:
```go
func main(){
	pow := make([]int, 10)

	for i := range pow {
		pow[i] = 1 << unit(i) // == 2**i
	}

	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
// 1
// 2
// 4
// 8
// 16
// 32
// 64
// 128
// 256
// 512
```
