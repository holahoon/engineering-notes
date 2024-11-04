Go does not have classes. However, you can define methods on types.
A method is a function with a special *receiver* argument.
The receiver appears in its own argument list between the `func` keyword and the method name.
In the below example, the `Abs` method has a receiver of type `Vertex` named `v`.

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X * v.X + v.Y * v.Y)
}

func main(){
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
	// 5
}
```

## Methods are functions

Remember: a method is just a function with a receiver argument.
Here's `Abs` written as a regular function with no change in functionality.
```go
type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X * v.X + v.Y * v.Y)
}

func main(){
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
	// 5
}
```

You can declare a method on non-struct types, too.
In the below example, we see a numeric type `MyFloat` with an `Abs` method.
You can only declare a method with a receiver whose type is defined in the same package as the method.
```go
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
}

func main(){
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
	// 1.4142135623730951
}
```