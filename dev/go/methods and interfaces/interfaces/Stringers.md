One of the most ubiquitous interfaces is `Stringer` defined by the `fmt` package.
This interface has a single method called `string()`.

```go
type Stringer interface {
	String() string
}
```

A `Stringer` is a type that can describe itself as a string. The `fmt` package (and many others) look for this interface to print values.

```go
package main

import "fmt"

type Person struct {
	Name string
	Age int
}

// Stringer
func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

// Not a stringer
func (p Person) Hello() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
	// {Arthur Dent 42} {Zaphod Beeblebrox 9001}
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"David Kim", 9001}
	fmt.Println(a, z)
	// Arthur Dent (42 years) David Kim (9001 years)
}
```

Here, we have a custom `String` method. The `Person` struct implements the `string()` method, which returns a formatted string representation of a `Person`.
Now, when `fmt.Sprintf(a, z)` is called in `main`, Go looks at the type of `a` and `z`. Since `a` and `z` has a `String()` method, Go calls `a.String()` and  `z.String()` instead of using the default struct representation. Hence we get `Arthur Dent (42 years) David Kim (9001 years)`.
If we remove the `String()` method or rename it to something like `Hello()`, calling `fmt.Sprintf(a, z)` would print something like `{Arthur Dent 42} {Zaphod Beeblebrox 9001}`, which is the default formatting for structs.

The `Stringer` interface is particularly helpful for:
- **Customized output**: You can control exactly how your types are printed, which is useful in applications where readability matters.
- **Debugging**: Implementing `String()` on types like `Person`, `Coordinate`, or `Address` can make it easier to understand logs and debug output.
- **Logging and reporting**: When you print a slice or map of custom types, each element will automatically use its `String()` method, making logs more informative.

### Example

Make the `IPAddr` type implement `fmt.Stringer` to print the address as a dotted quad.

For instance, `IPAddr{1, 2, 3, 4}` should print as `"1.2.3.4`.

```go
package main

import "fmt"

func () String() string {
	return fmt.Sprintf("%d.%d.%d.%d", i[0], i[1], i[2], i[3])
}

func main() {
	hosts := map[string]IPAddr{
		"loopback": {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}

	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
	// loopback: 127.0.0.1
	// googleDNS: 8.8.8.8
}
```