Return values may be given names, and if they are, then they are treated the same as if they were new variables defined at the top of the function.

Named return values are best thought of as a way to document the purpose of the returned values.

| A return statement without arguments returns the named return values. This is known as a "naked" return. Naked return statements should be used only in short functions. They can harm readability in longer functions.

Use naked returns if it's obvious what the purpose of the returned values is. Otherwise, use named returns for clarity.

```go
func getCoords(x, y int){
	// x and y are initialized with zero values
	return // automatically returns x and y
}
```

This is the same as:
```go
func getCoords() (int, int){
	var x int
	var y int
	return x, y
}
```

In the first example, `x` and `y` are the return values. At the end of the function, we could simply write `return` to return the values of those two variables, rather than writing `return x, y`.

## The Benefits of Named Returns

### Good for documentation (Understanding)

Named return parameters are great for documenting a function. We know what the function is returning directly from its signature, no need for comment.

Named return parameters are particularly important in longer functions with many return values.
```go
func calculator(a, b int) (mul, div int, err error) {
	if b === 0 {
		return 0, 0, errors.New("Can't divide by zero")
	}
	mul = a * b
	div = a / b
	return mul, div, nil
}
```

This is easier to understand than:
```go
func calculator(a, b int) (int, int, error) {
	...
	return mul, div, nil
}
```

We know the *meaning* of each return value just by looking at the function signature: `func calculator(a, b int) (mul, div int, err error)`

*Note:* `nil` is the zero value of an error.

### Less code (Sometimes)

If there are multiple return statements in a function, you don't need to write all the return values each time, though you *probably* should.

When you choose to omit return values, it's called a *naked* return. Naked returns should only be used in short and simple functions.
