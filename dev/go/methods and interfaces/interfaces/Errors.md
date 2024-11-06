Go programs express error state with `error` values.

The `error` type is a built-in interface similar to `fmt.Stringer`:
```go
type error interface {
	Error() string
}
```
(As with `fmt.Stringer`, the `fmt` package looks for the `error` interface when printing values.)

Functions often return an `error` value, and calling code should handle errors by testing whether the error equals `nil`.

```go
i, err := strconv.Atoi("42")
if err != nil {
	fmt.Printf("Couldn't convert number: %v\n", err)
	return
}
fmt.Println("Converted integer: ", i)
```

A nil `error` denotes success; a non-nil `error` denotes failure.

Example
```go
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s", e.When, e.What)
}

func run() error {
	return &MyError{time.Now(), "it didn't work..."}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
		// at 2009-11-10 23:00:00 +0000 UTC m=+0.000000001, it didn't work
	}
}
```