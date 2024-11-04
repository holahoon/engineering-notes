An *interface type* is defined as a set of method signatures.

A value of interface type can hold any value that implements those methods.

**Note**: There is an error in the example code on line `a = v`.  `Vertex` (the value type) doesn't implement `Abser` because the `Abs` method is defined only on `*Vertex` (the pointer type).
```go
type Abser interface {
	Abs() float64
}

func main(){
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser

	// In the following line, v is a Vertex (not *Vertex)
	// and does NOT implement Abser.
	a = v // remove this line for compiler to work

	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

// 5
```

Here's what's going on.
1. Interface implementation requirement:
	- The `Abser` interface requires a single method: `Abs() float64`.
	- The method `Abs()` for `Vertex` is defined with a pointer receiver: `func (v *Vertex) Abs() float64`.
	- This means only a `*Vertex` (a pointer to a `Vertex`) implements `Abser`, not a `Vertex` value.
2. Assignment error:
	- When you write `a = v`, you are trying to assign a `Vertex` value to the `Abser` interface. However, because `Vertex` itself does not implement `Abs()` (only `*Vertex` does), the compiler raises an error, as it expects `a` to hold something that satisfies the `Abser` interface.
3. Solution:
	- To fix this, change `a = v` to `a = &v` (like the above) to assign a pointer to `Vertex` rather than the `Vertex` value itself. This ensures that the `Abser` interface is satisfied.

## Interfaces are implemented implicitly

A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.
Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

```go
type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"DK"}
	i.M() // DK
}
```
