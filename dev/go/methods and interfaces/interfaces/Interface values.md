Under the hood, interface values can be thought of as tuple of a value and a concrete type:
```go
(value, type)
```

An interface value holds a value of a specific underlying concrete type.
Calling a method on an interface value executes the method of the same name on its underlying type.

```go
type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	fmt.Println(t.S)
}

type F float64

func (f F) M() {
	fmt.Println(f)
}

func main() {
	var i I

	i = &T{"Hello"}
	describe(i)
	i.M()
	// (&{Hello}, *main.T)
	// Hello

	i = F(math.Pi)
	describe(i)
	i.M()
	// (3.141592653589793, main.F)
	// 3.141592653589793
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## Interface values with nil underlying values

If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.

In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver (as the method `M` in the below example.)

Note that an interface value that holds a nil concrete value is itself non-nil.

```go
type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

func main() {
	var i I

	var t *T
	i = t
	describe()
	i.M()
	// (<nil>, *main.T)
	// <nil>

	i = &T{"DK"}
	describe(i)
	i.M()
	// (&{hello}, *main.T)
	// hello
}

func describe(){
	fmt.Printf("(%v, %T)\n", i, i)
}
```
