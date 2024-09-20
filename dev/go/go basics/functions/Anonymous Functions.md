Anonymous functions are true to form in that they have *no name*. They're useful when defining a function that will only be used once or to create a quick [closure](https://en.wikipedia.org/wiki/Closure_(computer_programming)).

Let's say we have a function `conversions` that accepts another function, `converter` as input:
```go
func conversions(converter func(int) int, x, y, z int) (int, int, int) {
	convertedX := converter(x)
	convertedY := converter(y)
	convertedZ := converter(z)
	return convertedX, convertedY, convertedZ
}
```

We *could* define a function normally and then pass it in by name... but it's usually easier to just define it anonymously:
```go
func main(){
	newX, newY, newZ := conversions(func(a int) int {
		return a + a
	}, 1, 2, 3)
	// newX is 2, newY is 4, newZ is 6

	newX, newY, newZ = conversions(func(a int) int {
		return a * a
	}, 1, 2, 3)
	// newX is 1, newY is 4, newZ is 9
}
```

Here's an example:
```go
package main

import "fmt"

func printReports(intro, body, outro string){
	printCostReport(func(m string) int {
		return len(m) * 2
	}, intro)
	// Message: "Welcome to the Hotel California" Cost: 62 cents
	
	printCostReport(func(m string) int {
		return len(m) * 2
	}, body)
	// Message: "Such a lovely place" Cost: 57 cents
	
	printCostReport(func(m string) int {
		return len(m) * 2
	}, outro)
	// Message: "Plenty of room at the Hotel California" Cost: 152 cents
}

func main(){
	printReports(
		"Welcome to the Hotel California",
		"Such a lovely place",
		"Plenty of room at the Hotel California",
	)
}

func printCostReport(costCalculator func(string) int, message string){
	cost := costCalculator(message)
	fmt.Printf('Message: "%s" Cost: %v cents', message, cost)
	fmt.Println()
}
```

