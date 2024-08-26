
Go's return values may be named. If so, they are treated as variables defined at the top of the function.

A `return` statement without arguments returns the named return values. This is known as a "naked" return.

Naked return statements should be used only in short functions. They can harm readability in longer functions.

```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

// 7 10
```
This creates a variable called `x` and `y` in the `split` function. It assigns the "zero" value - depending on the type, for example `int`s are 0 and for `string`s it it `""`.
This display in the Go Doc for your function so it can make the intent of the code clearer.

Notice that the function name starts with a lowercase letter. In Go, public functions start with a capital letter, and private ones start with a lowercase letter. We don't want the internals of our algorithm exposed to the world, so we make this function private.
