A `defer`  statement defers the execution of a function until the surrounding function returns.
The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```go
func main(){
	defer fmt.Println("world")

	fmt.Println("hello")
}
// hello
// world
```

## Stacking defers

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.
Here's a [blog post](https://go.dev/blog/defer-panic-and-recover) that talks about defer statements.

```go
func main(){
	fmt.Println("counting")

	for i := 0; i < 3; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}

// counting
// done
// 2
// 1
// 0
```