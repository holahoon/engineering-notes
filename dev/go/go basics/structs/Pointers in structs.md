Struct fields can be accessed through a struct pointer

To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`.
However, that notation is cumbersome, so the language permits us instead to write just `p.X`, without the explicit dereference.

```go
package main
import "fmt"

type Vertex struct {
	X int
	Y int
}

func main(){
	v := Vertex{1, 2}
	p := &v
	p.X = 77
	fmt.Println(v)
	// {77 3}
}
```
