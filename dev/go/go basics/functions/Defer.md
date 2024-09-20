The `defer` keyword is a fairly unique feature of Go. It allows a function to be executed automatically *just before* its enclosing function returns. The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred functions are typically used to clean up resources that are no longer being used. Often to close database connections, file handlers and the like.

For example,
```go
func GetUsername(dstName, srcName string) (username string, err error){
	// Open a connection to a database
	conn, _ := db.Open(srcName)

	// Close the source file *anywhere* the GetUsername function returns
	defer conn.Close()

	username, err = db.FetchUser()
	if err != nil {
		// The defer statement is auto-executed if we return here
		return "", err
	}

	// The defer statement is auto-executed if we return here
	return username, nil
}
```

In the above example, the `conn.Close()` function is not called here:
```go
defer conn.Close()
```

It's called
```go
// here
return "", err
// or here
return username, nil
```

Depending on whether the `FetchUser` function errored.

Defer is a great way to make sure that something happens before a function exits, even if there are multiple return statements, a common occurrence in Go.