In Go, an interface value consists of both concrete type and a value (an instance of that type). When you assign `nil` to an interface, both the type and the value within that interface are `nil`, which is what's known as a **nil interface value**.

A nil interface value holds neither value or concrete type.
Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which *concrete* method to call.

```go
type I interface {
	M()
}

func main(){
	var i I
	describe(i)
	i.M()
	// (<nil>, <nil>)
}

func describe(i) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

We get the following error:
```
panic: runtime error: invalid memory address or nil pointer dereference
[signal SIGSEGV: segmentation violation code=0x1 addr=0x0 pc=0x490339]
```

Here, we declare a variable `i` of interface type `I`, but don't assign anything to it. At this point, `i` is a **nil interface value**.
When calling the `describe(i)` function, it prints the internal representation of `i`, which is `(nil, <nil>)`.
The `i.M()` attempts to call the method `M()` on `i`.
To fix this, we need to:
```go
type I interface {
    M()
}

type T struct{}

func (T) M() {
    fmt.Println("M method called on T")
}

func main() {
    var i I
    describe(i)      // Prints (nil, <nil>), showing that `i` is still a nil interface.
    
    i = T{}          // Assign a value of type `T`, which implements `M()`, to `i`.
    describe(i)      // Now `i` holds a concrete type `T` and is no longer nil.
    i.M()            // Calls `M()` on `T`, avoiding a runtime error.
}
...
```