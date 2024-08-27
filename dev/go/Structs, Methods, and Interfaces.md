
## Struct
[doc](https://go.dev/ref/spec#Struct_types)
A struct is a sequence of names elements, called fields, each of which has a name and a type. Field names may be explicitly (IdentifierList) or implicitly (EmbeddedField). Within a struct, non-blank field names must be unique.
Basically, it's just a named collection of fields where you can store data.

```go
// shapes.go
package shapes

type Rectangle struct {
	Width float64
	Height float64
}

func Perimeter(rectangle Rectangle) float64 {
	return 2 * (rectangle.Width + rectangle.Height)
}

func Area(rectangle Rectangle) float64 {
	return rectangle.Width * rectangle.Height
}
```

We can write test like:
```go
// shapes_test.go
package shapes

import "testing"

func TestPerimeter(t *testing.T) {
	rectangle := Rectangle{10.0, 10.0}
	got := Perimeter(rectangle)
	want := 40.0

	if got != want {
		t.Errorf("Got %.2f, want %.2f", got, want)
	}
}

func TestArea(t *testing.T) {
	got := Area(Rectangle{12.0, 6.0})
	want := 72.0

	if got != want {
		t.Errorf("Got %.2f, want %2.f", got, want)
	}
}
```

Btw, we can collapse fields that have the same type:
```go
type Rectangle struct {
	Width, Height float64
}
```

## Method
[doc](https://go.dev/ref/spec#Method_declarations)
A method is a function with a *receiver*. A method declaration binds an identifier, the *method* name, to a method, and associates the method with the receiver's *base type*.

Basically, when we call `t.Errorf`, we are calling the method `Errorf` on the instance of our `t`(`testing.T`).

Methods are very similar to functions but they are called by invoking them on an instance of a particular type. Where you can just call function wherever you like, such as `Area(rectangle)` you can only call methods on "things".

We can take our existing code and refactor it to use methods.
```go
// shaes_test.go
package shapes

import "testing"

func TestArea(t *testing.T) {

	t.Run("rectangles", func(t *testing.T) {
		rectangle := Rectangle{12, 6}
		got := rectangle.Area()
		want := 72.0

		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	})

	t.Run("circles", func(t *testing.T) {
		circle := Circle{10}
		got := circle.Area()
		want := 314.1592653589793

		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	})
}
```

```go
// shapes.go
package shapes

import "math"

type Rectangle struct {
	Width float64
	Height float64
}

func (r Rectangle) Area() float64 {
	return r.Width * r.Height
}

type Circle struct {
	Radius float64
}

func (c Circle) Area() float64 {
	return math.Pi * c.Radius * c.Radius
}
```

The syntax for declaring methods is almost the same as functions and that's because they're so similar. The only difference is the syntax of the method receiver.
```go
func (receiverName ReceiverType) MethodName(args)
```

When your method is called on a variable of that type, you get your reference to its data via the `receiverName` variable. In many other programming languages, this is done implicitly and you access the receiver via `this`.

Btw, it's a convention in Go to have the receiver variable be the first letter of the type.
```go
r Rectangle
```

## Interface
[doc](https://go.dev/ref/spec#Interface_types)
An interface type defines a *type set*. A variable of interface type can store a value of any type that is in the type set of the interface. Such a type is said to [implement the interface](https://go.dev/ref/spec#Implementing_an_interface). The value of an uninitialized variable of interface type if nil.
It's a very powerful concept in statically typed languages like Go because they allow you to make functions that can be used with different types and create highly-decoupled code whilst still maintaining type-safety.

We can refactor our code since there are some duplication in the test code.
Al we want to do is take a collection of *shapes*, call the `Area()` method on them and then check the result.
We want to be able to write some kind of `checkArea` function that we can pass both `Rectangle`s and `Circle`s too, but fail to compile if we try to pass something that isn't a shape.
With Go, we can codify this intent with **interfaces**.

Very similar to struct, but instead of defining fields, we define a "method set". A method set is a list of methods that a type must have in order to "implement" the interface.

```go
// shapes_test.go
func TestArea(t *testing.T) {
	checkArea := func(t testing.TB, shape Shape, want float64) {
		t.Helper()
		got := shape.Area()
		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	}

	t.Run("rectangles", func(t *testing.T) {
		rectangle := Rectangle{12, 6}
		checkArea(t, rectangle, 72.0)
	})

	t.Run("circles", func(t *testing.T) {
		circle := Circle{10}
		checkArea(t, circle, 314.1592653589793)
	})
}
```
Here, we create a helper function. This accepts a `Shape`. Now we need to go back to our `shape.go` file and tell Go what a `Shape` is.

```go
// shapes.go
type Shape interface {
	Area() float64
}
...
```
We created a new `type` just like we did with `Rectangle` and `Circle` but this time, it's an `interface` rather than a `struct`.
In Go, **interface resolution is implicit**. If the type you pass in matches what the interface is asking for, it will compile.

### Decoupling
Notice how the helper function does not need to concern itself with whether the shape is a `Rectangle` or a `Circle` or a `Triangle`. By declaring an interface, the helper is *decoupled* from the concrete types and only has the method it needs to do its job.

This kind of approach of using interfaces to declare **only what you need** is very important in software design.

### Refactor using Table driven tests
[doc](https://go.dev/wiki/TableDrivenTests)
Table driven tests are useful when you want to build a list of test cases that can be tested in the same manner.

```go
// shape_test.go
func TestArea(t *testing.T) {

	areaTests := []struct {
		shape Shape
		want  float64
	}{
		{Rectangle{12, 6}, 72.0},
		{Circle{10}, 314.1592653589793},
	}

	for _, tt := range areaTests {
		got := tt.shape.Area()
		if got != tt.want {
			t.Errorf("got %g want %g", got, tt.want)
		}
	}
}
```
Here, we create an "anonymous struct", `areaTests`. All we are doing is declaring a slice of structs by using `[]struct` with two fields, the `shape` and the `want`. Then we fill the slice with cases.
We then iterate over them just like we do with any other slice, using the struct fields to run our tests.
Now, this makes it very easy for a developer to introduce a new shape, implement `Area` and then add it to the test cases. Also, if a bug is found  `Area`, it's very easy to add a new test case to exercise it before fixing it.

Keep in mind that table driven tests can be a great item, but be sure that you have a need for the extra noise in the tests. They are a great fit when you wish to test various implementations of an interface, or if the data being passed in to a function has lots of different requirements that need testing.

We can now easily add in a new shape: `Triangle`:
```go
// shape_test.go
func TestArea(t *testing.T) {

	areaTests := []struct {
		shape Shape
		want  float64
	}{
		{Rectangle{12, 6}, 72.0},
		{Circle{10}, 314.1592653589793},
		{Triangle{12, 6}, 36.0}, // <- new
	}

	for _, tt := range areaTests {
		got := tt.shape.Area()
		if got != tt.want {
			t.Errorf("got %g want %g", got, tt.want)
		}
	}
}
```

```go
// shape.go

// Define a Triangle struct
type Triangle struct {
	Base   float64
	Height float64
}

// Define a method
func (t Triangle) Area() float64 {
	return (t.Base * t.Height) / 2
}
```

#### We can refactor even further.
When reading the below values, it's not immediately clear what all the numbers represent.
```go
{Rectangle{12, 6}, 72.0},
{Circle{10}, 314.1592653589793},
{Triangle{12, 6}, 36.0},
```

So far, we've only seen creating instances of structs `MyStruct{val1, val2}`, but you can optionally name the fields:
```go
{shape: Rectangle{Width: 12, Height: 6}, want: 72.0},
{shape: Circle{Radius: 10}, want: 314.1592653589793},
{shape: Triangle{Base: 12, Height: 6}, want: 36.0},
```
Much better to read.

#### Make sure the test output is helpful

With what we currently have, if one of the test fails, it's going to be hard to tell which case failed, and we would need to manually look through the cases to find out which case failed.

We can change our error message into `%#v got %g, want %g`. This `%#v` format string prints out the struct with the values in its field.
Let's also rename the `want` to something like `hasArea` so it's more descriptively clear.
Oh, let's also wrap each test to use `r.Run` and name the test cases.

```go
// shape_test.go

func TestArea(t *testing.T) {
	areaTests := []struct {
		name    string
		shape   Shape
		hasArea float64
	}{
		{name: "Rectangle", shape: Rectangle{Width: 12, Height: 6}, hasArea: 72.0},
		{name: "Circle", shape: Circle{Radius: 10}, hasArea: 314.1592653589793},
		{name: "Triangle", shape: Triangle{Base: 12, Height: 6}, hasArea: 36.0},
	}

	for _, tt := range areaTests {
		// using tt.name from the case to use it as the `t.Run` test name
		t.Run(tt.name, func(t *testing.T) {
			got := tt.shape.Area()
			if got != tt.hasArea {
				t.Errorf("%#v got %g, want %g", tt.shape, got, tt.hasArea)
			}
		})
	}
}
```

Now, if a test case fails, the message will be more clear:
```
--- FAIL: TestArea (0.00s)
    --- FAIL: TestArea/Rectangle (0.00s)
        shapes_test.go:51: shapes.Rectangle{Width:12, Height:6} got 72, want 72.1
```
