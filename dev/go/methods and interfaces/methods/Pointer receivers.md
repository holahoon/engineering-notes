You can declare methods with pointer receivers.
This means the receiver type has the literal syntax `*T` for some type `T`. (Also, `T` cannot itself be a pointer such as `*int`).

For example, the `Scale` method here is defined on `*Vertex`.
Methods with pointer receivers can modify the value to which the receiver points (as `Scale` does here). Since methods often need to modify their receiver, pointer receivers are more common than value receivers.

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X * v.X + v.Y * v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main(){
	v := Vertex{3, 4}
	fmt.Println(v.Abs()) // 5
	v.Scale(10)
	fmt.Println(v.Abs()) // 50
}
```
If you remove the `*` from the declaration of the `Scale` function, the result will still be `5`.
With a value receiver, the `Scale` method operates on a copy of the original `Vertex` value. (This is the same behavior as for any other function argument). The `Scale` method must have a pointer receiver to change the `Vertex` value declared in the `main` function.

## Pointers and functions

Here we see the `Abs` and `Scale` methods rewritten as functions.
```go
type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X * v.X + v.Y * v.Y)
}

func Scale(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main(){
	v := Vertex{3, 4}
	Scale(&v, 10)
	fmt.Println(Abs(v)) // 50
}
```
Notice that if we remove the `*` from `Scale` function, it compile errors.

## Methods and pointer indirection

Comparing the previous two examples, the function with pointer argument must take a pointer:
```go
var v Vertex
ScaleFunc(v, 5) // Compile error!
ScaleFunc(&v, 5) // OK
```

while methods with pointer receivers take either a value or a pointer as the receiver when they are called:
```go
var v Vertex
v.Scale(5) // OK
p := &v
p.Scale(10) // OK
```

For the statement `v.Scale(5)`, even though `v` is a value and not a pointer, the method with the pointer receiver is called automatically. That is as convenience, Go interprets the statement `v.Scale(5)` as `(&v).Scale(5)` since the `Scale` method has a pointer receiver.

```go
type Vertex struct {
	X, Y float64
}

// A method defined on the struct pointer
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

// Standalone function that takes a pointer as an argument
func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main(){
	v := Vertex{3, 4}
	v.Scale(2)
	scaleFunc(&v, 10)

	p := &Vertex{4, 3}
	p.Scale(3)
	ScaleFunc(p, 8)

	fmt.Println(v, p)
}
```

Here's a code breakdown:

We first defined the struct:
```go
type Vertex struct {
	X, Y float64
}
```

Then we declared a method with pointer receiver:
```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
- This method, `Scale`, is defined on a pointer receiver `*Vertex`.
- It takes a float parameter `f` and scales `X` and `Y` by multiplying each with `f`.
- Since `v` is a pointer to `Vertex`, it can modify the `Vertex` instance it is called on directly.

Standalone function with pointer parameter:
```go
func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
- `ScaleFunc` is a regular function (not a method) that takes a pointer to `Vertex` (`*Vertex`) as its first argument and a float `f` as the second.
- Like the method, it scales `X` and `Y` by multiplying them with `f`.
- Since `v` is a pointer, this function also directly modifies the original `Vertex` instance.

Using the method and function in `main`:
```go
func main(){
	v := Vertex{3, 4}
	v.Scale(2)
	scaleFunc(&v, 10)

	p := &Vertex{4, 3}
	p.Scale(3)
	ScaleFunc(p, 8)

	fmt.Println(v, p)
}
```
- in `main`, we create and modify two `Vertex` instances using both the method and the function.

The first example with `v`:
```go
v := Vertex{3, 4}
```
- `v` is a `Vertex` initialized with `X = 3` and `Y = 4`.

```go
v.Scale(2)
```
- The method `Scale` is called on `v`, scaling `X` and `Y` by `2`. Since `Scale` is defined with a pointer receiver (`*Vertex`), `v` is implicitly converted to `&v` when calling `Scale`.
- After this call, `V.X` becomes `3 * 2 = 6`, and `v.Y` becomes `4 * 2 = 8`. So, `v` is now `{6, 8}`.

```go
ScaleFunc(&v, 10)
```
- `ScaleFunc` is called with `&v` explicitly passed as a pointer. This scales `X` and `Y` by `10`.
- After this call, `v.X` becomes `6 * 10 = 60`, and `v.Y` becomes `8 * 10 = 80`. So, `v` is now `{60, 80}`.

The second example with `p`:
```go
p := &Vertex{4, 3}
```
- `p` is a pointer to a `Vertex` initialized with `X = 4` and `Y = 3`.

```go
p.Scale(3)
```
- `Scale` is called on `p`, scaling `X` and `Y` by `3`. Since `p` is already a pointer, there is no conversion needed.
- After this call, `p.X` becomes `4 * 3 = 12`, and `p.Y` becomes `3 * 3 = 9`. So, `p` is now `{12, 9}`.

```go
ScaleFunc(p, 8)
```
- `ScaleFunc` is called with `p` passed as a pointer, scaling `X` and `Y` by `8`.
- After this call, `p.X` becomes `12 * 8 = 96`, and `p.Y` becomes `9 * 8 = 72`. So, `p` is now `{96, 72}`.

Side note: Here's a summary of the differences between two examples:

The main difference between using `p := &Vertex{}` (a pointer to a `Vertex` instance) and `v := Vertex{}` (a `Vertex` value) comes down to **how the instance is passed and accessed**, **efficiency**, and **flexibility**. Here are the benefits and scenarios where each approach might be preferred:
#### 1. **Efficiency and Memory Usage**
- **Using `p := &Vertex{}` (Pointer)**:
    - When you pass `p` (a pointer) to a function or method, only the pointer’s address (usually 8 bytes on a 64-bit system) is passed. This is efficient because no copies of the actual struct are made.
    - If `Vertex` is a large struct, using a pointer avoids the need to copy the entire struct each time you pass it, which can save memory and improve performance.
- **Using `v := Vertex{}` (Value)**:
    - When you pass `v` (the actual value) to a function, a copy of the entire `Vertex` struct is made. This can be more memory-intensive if the struct is large or if you’re passing it around frequently.
    - For small structs, like `Vertex` with just two fields, this is typically not an issue. But for larger structs, pointers are more efficient.
#### 2. **Mutability**
- **Using `p := &Vertex{}` (Pointer)**:
    - With a pointer, any changes made inside a function or method will directly affect the original `Vertex` instance. This is useful when you need to **mutate the original instance**.
    - In the `Scale` method, for instance, changes to `v.X` and `v.Y` affect the instance that `p` points to. So, you can modify the same instance across multiple functions without returning or reassigning it.
- **Using `v := Vertex{}` (Value)**:
    - When using a value receiver or passing `v` by value, modifications do not affect the original instance; they only affect the copy.
    - This can be beneficial if you need to **ensure immutability** or protect the original data. You can safely pass the value around without worrying about accidental modifications.
#### 3. **Method Receivers and Automatic Conversion**
- **Using `p := &Vertex{}` (Pointer)**:
    - If you define methods with pointer receivers, such as `func (v *Vertex) Scale(f float64)`, they expect a pointer to `Vertex`.
    - Calling these methods on `p` is straightforward since `p` is already a pointer. Methods with pointer receivers can modify the instance’s internal state, making them suitable for mutable operations.
- **Using `v := Vertex{}` (Value)**:
    - If you have a `Vertex` value `v` and want to call a method with a pointer receiver (e.g., `v.Scale(2)`), Go automatically converts `v` to `&v` when calling the method.
    - This automatic conversion works, but only for methods. If you want to pass `v` to a function like `ScaleFunc` that expects `*Vertex`, you need to explicitly pass `&v`.
#### 4. **Flexibility in Initialization and Usage**
- **Using `p := &Vertex{}` (Pointer)**:
    - Pointers offer flexibility because they can be `nil`, meaning they don’t have to point to a valid `Vertex` instance. This can be useful for delayed initialization or when working with collections where some items might be absent (`nil` pointers).
    - `nil` pointers can be used to signify optional data or the absence of a `Vertex` without needing a separate boolean flag.
- **Using `v := Vertex{}` (Value)**:
    - A `Vertex` value cannot be `nil`—it always holds an instance of `Vertex` with default zero values. This is beneficial if you need to avoid `nil` checks, as you’re guaranteed that `v` is a valid instance.
    - If you only need an instance with default values and no nil-checking, initializing by value is simpler.
### Summary: When to Use Each
- Use **`p := &Vertex{}`** (pointer) when:
    - You want to modify the `Vertex` instance directly.
    - The struct is large, and you want to avoid unnecessary copies.
    - You need the flexibility of `nil` values to represent an optional or uninitialized state.
- Use **`v := Vertex{}`** (value) when:
    - The struct is small, and copying it won’t impact performance.
    - You want to avoid unintentional changes and keep `Vertex` immutable in certain functions.
    - You want to simplify initialization without handling `nil`.

Thanks ChatGPT

## Methods and pointer indirection (2)

The equivalent thing happens in the reverse direction.

Functions that take a value argument must take a value of that specific type:
```go
var v Vertex
fmt.Println(AbsFunc(v)) // OK
fmt.Println(AbsFunc(&v)) // Compile error!
```

while methods with value receivers take either a value or a pointer as the receiver when they are called:
```go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```
In this case, the method call `p.Abs()` is interpreted as `(*p).Abs()`.

Example:
```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func AbsFunc(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main(){
	v := Vertex{3, 4}
	fmt.Println(v.Abs()) // 5
	fmt.Println(AbsFunc(v)) // 5
	fmt.Println(AbsFunc(*v)) // invalid operation: cannot indirect v (variable of type Vertex)

	p := &Vertex{4, 3}
	fmt.Println(p.Abs()) // 5
	fmt.Println(AbsFunc(*p)) // 5
	fmt.Println(AbsFunc(p)) // cannot use p (variable of type *Vertex) as Vertex value in argument to AbsFunc
}
```

## Choosing a value or pointer receiver

There are two reasons to use a pointer receiver.

- The first is so that the method can modify the value that its receiver points to.
- The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.

In the below example, both `Scale` and `Abs` are methods with receiver type `*Vertex`, even though the `Abs` method doesn't need to modify its receiver.

In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both.

```go
type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main(){
	v := &Vertex{3, 4}
	fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
	v.Scale(5)
	fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
}

// Before scaling: {X:3 Y:4}, Abs: 5
// After scaling: {X:15 Y:20}, Abs: 25
```
