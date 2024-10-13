Unlike Python, Go is *not* function-scoped, it's [block-scoped](https://go.dev/ref/spec#Declarations_and_scope). Variables declared inside a block are only accessible within that block (and its nested blocks). There's also the package scope, which will be covered later), but for now, you can think of it as the outermost, nearly global scope.

```go
package main

// scoped to the entire "main" package (basically global)
var age = 19

func sendEmail(){
	// scoped to the "sendEmail" function
	name := "Jon Snow"

	for i := 0; i < 5; i++ {
		// scoped to the "for" body
		email := "snow@winterfell.net"
	}
}
```

Blocks are defined by curly braces `{}`. New blocks are created for:
- Functions
- Loops
- If statements
- Switch statements
- Select statements
- Explicit block

It's a bit unusual, but occasionally you'll see a plain old explicit block. It exists for no other reason than to create a new scope.
```go
package main

func main(){
	{
		age := 19
		// this is okay
		fmt.Println(age)
	}

	// this is not okay
	// the age variable is out of scope
	fmt.Println(age)
}
```
