Let's write a function that greets someone. Here's a sample code:
```go
func Greet(name string) {
	fmt.Printf("Hello, %s", name)
}
```

But how do we test this function? Calling `fmt.Printf` prints to stdout, and there's no way for us to capture using the testing framework.

What we need to do is to be able to **inject** (which is just a fancy word for pass in) in the dependency of printing.

Our **function does not need to care where or how the printing happens, so we should accept an interface rather than a concrete type**.

If you look at the source code of `fmt.Printf`, you can see a way for us to hook in
```go
// It returns the number of bytes written and any write error encountered.
func Printf(format string, a ...interface{}) (n int, err error) {
	return Fprintf(os.Stdout, format, a...)
}
```
Interesting 🤔 Under the hood `Printf` just calls `Fprintf` passing in `os.Stdout`.
So what is `os.Stdout`? What does `Fprintf` expect to get passed to it for the first argument?

```go
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error) {
	p := newPrinter()
	p.doPrintf(format, a)
	n, err = w.Write(p.buf)
	p.free()
	return
}
```

An `io.writer`
```go
type Writer interface {
	Write(p []byte) (n int, err error)
}
```

From this we can infer that `os.Stdout` implements `io.Writer`; `Printf` passes `os.Stdout` to `Fprintf` which expects an `io.Writer`.

So we know under the covers we're ultimately using `Writer` to send our greeting somewhere. Let's use this existing abstraction to make our code testable and more reusable.

## Wrap up

- **Test our code**: If you can't test a function *easily*, it's usually because of dependencies hard-wired into a function or global state. If you have a global database connection pool for instance that is used by some kind of service layer, it is likely going to be difficult to test and they will be slow to run. DI will motivate you to inject in a database dependency (via an interface) which you can then mock out with something you can control in your tests.
- **Separate our concerns**: decoupling *where the data goes from how to generate it*. If you ever feel like a method/function has too many responsibilities (generating *data and writing* to a db? handling HTTP request *and* doing domain level logic?) DI is probably going to be the tool you need.
- **Allow our code to be re-used in different contexts**: The first "new" context our code can be used in is inside tests. But further on if someone wants to try something new with your function, they can inject their own dependencies.


