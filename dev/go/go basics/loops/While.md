For is Go's "while". At that point you can drop the semicolons: C's `while` is spelled `for` in Go.
```go
package main
import "fmt"

func main(){
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
	// 1024
}
```

## Forever
If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.
```go
package main
import "fmt"

func main(){
	for {
	}
}
```
It breaks
