Here's an example:
```go
func getCreator(os string) string {
	var creator string
	switch(os){
		case "linux":
			creator = "Linus Torvalds"
		case "windows":
			creator = "Bill Gates"
		case "mac":
			creator = "A Steve"
		default:
			creator = "Unknown"
	}
	return creator
}
```

Notice that in Go, the `break` statement is not required at the end of a `case` to stop it from falling through to the next `case`. The `break` statement is implicit in Go.

If you do want a `case` to fall through to the next `case`, you can use the `fallthrough` keyword.
```go
func getCreator(os string) string {
    var creator string
    switch os {
    case "linux":
        creator = "Linus Torvalds"
    case "windows":
        creator = "Bill Gates"

    // all three of these cases will set creator to "A Steve"
    case "Mac OS":
        fallthrough
    case "Mac OS X":
        fallthrough
    case "mac":
        creator = "A Steve"

    default:
        creator = "Unknown"
    }
    return creator
}
```

Another important difference is that Go's switch cases need not be constants, and the values involved need not be integers.

```go
package main
import (
	"fmt"
	"runtime"
)

func main(){
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
		case "darwin":
			fmt.Println("OS X.")
		case "linux":
			fmt.Println("Linux.")
		default:
			// freebsd, openbsd, plan9, windows...
			fmt.Printf("%s.\n", os)
	}
	// Go runs on Linux.
}
```

## Switch evaluation order

Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

For example,
```go
switch i {
	case 0:
	case f():
}
```
does not call `f` if `i == 0`.

## Switch with no condition

Switch without a condition is the same as `switch true`.
This construct can be a clean way to write long if-then-else chains.
```go
func main(){
	t := time.Now()
	switch {
		case t.Hour() < 12:
			fmt.Println("Good morning!")
		case t.Hour() < 17:
			fmt.Println("Good afternoon.")
		default:
			fmt.Println("Good evening.")
	}
}
```