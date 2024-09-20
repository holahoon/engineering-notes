A function can return a value that the caller doesn't care about. We can explicitly ignore variables by using an underscore, or more precisely, the [blank identifier](https://go.dev/doc/effective_go#blank.

For example:
```go
func getPoint(x, y int){
	return 3, 4
}

x, _ := getPoint()
```
We ignore the `y` value here.

Even though `getPoint()` returns two values, we can capture the first one and ignore the second. In Go, the blank identifier isn't just a convention; it's a real language feature that completely discards the value.

## Why might you ignore a return value?
Maybe a function called `getCircle` returns the center point and the radius, but you only need the radius for your calculation. In that case, you can ignore the center point variable.

The Go compiler will **throw an error** if you have any unused variable declarations in your code, so you need to ignore anything you don't intend to use.
