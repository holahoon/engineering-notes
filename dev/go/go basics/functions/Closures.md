A closure is a function that references variables from outside its own function body. The function may access and *assign* to the referenced variables.

The below example, the `concatter()` function returns a function that has reference to an *enclosed* `doc` value. Each successive call to `harryPotterAggregator` mutates that same `doc` variable.

```go
func concatter() func(string) string {
	doc := ""
	return func(word string) string {
		doc += word + " "
		return doc
	}
}

func main() {
	greet := concatter()
	greet("Hello")
	greet("DK")

	fmt.Println(greet("!"))
	// Hello DK !
}
```

### Example

Let's write a function keeps running a total of the `sum` variable within a closure.

```go
package main

func adder() func(int) int {
	sum := 0
	return func(count int) int {
		sum += count
		return sum
	}
}
```