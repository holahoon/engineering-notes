## Packages

A package is a file or a group of files with namespace and some code or related code. This means that a package could live in a file such as `math.go` or across multiple files such as `add.go`, `subtract.go`, `multiply.go`, this file will be namespaced as `package math` for example.
A package has a surrounding directory like:
```
math
|-- math.go

or
math
|-- subtract.go
|-- add.go
|-- multiply.go
```

The content of `math.go` could look something like:
```go
package math

func Add(numbers ...int) int {
	sum := 0
	for _, number := range numbers {
		sum += number
	}
	return sum
}

func Subtract(x, y int) int {
	return x - y
}

func Multiply(numbers ...int) int {
	result := 1
	for _, number := range numbers {
		reult *= number
	}
	return result
}
```

Or we can just split all these functions into multiple files by making three files-`add.go`, `subtract.go`, `multiply.go` with each respective function in it.

All the files have a common denominator -- they all share the same package namespace `math`.

### Exported and Local functions/methods
The idea behind writing packages in Golang is to create modularized pieces of code that can be shared across your projects or with other developers for use in their own projects.

Of course, we need to account for security measures. Some languages provide constructs like `public`, `private`, `protected` to help with this, but in Golang, the concepts of `public` and `private` APIs are imported from the way we name our functions, methods, and properties:
```go
package math

import "fmt"

func Multiply(numbers, ...int)int {
	result := multiply(numbers...)
	fmt.Println("Multiplying %v, gives us %v", numbers, result)
}

func multiply(numbers ...int)int {
	result := 1
	for _, number := range numbers {
		result *= number
	}
	return result
}
```

When you name a function in full lowercase, it is localized and cannot be accessed of this package. On the other hand, anything that starts with a capital letter can be accessed from within and outside of the package where it was defined/declared.


