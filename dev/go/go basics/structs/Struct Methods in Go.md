
While Go is **not** object-oriented, it does support methods that can be defined on structs. Methods are just functions that have a receiver. A receiver is a special parameter that syntactically goes *before* the name of the function.

```go
type rect struct {
	width int
	height int
}

// area has a receiver of (r rect)
// rect is the struct
// r is the placeholder
func (r rect) area() int {
	return r.width * r.height
}

var r = rect{
	width: 5,
	height: 10,
}

fmt.Println(r.area())
// prints 50
```

A receiver is just a special kind of function parameter. In the example above, the `r` in `(r rect)` could just as easily have been `rec` or even `x`, `y` or `z`. By convention, Go code will often use the first letter of the struct's name.

Receivers are important because they allow us to define interfaces that our structs (and other types) can implement.