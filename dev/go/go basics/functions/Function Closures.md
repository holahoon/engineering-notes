Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.
For example, the `adder` function returns a closure. Each closure is bound to its own `sum` variable.
```go
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main(){
	pos, neg := adder(), adder()

	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2 * i),
		)
	}
}

// 0 0
// 1 -2
// 3 -6
// 6 -12
// 10 -20
// 15 -30
// 21 -42
// 28 -56
// 36 -72
// 45 -90
```

## A quick exercise: Fibonacci closure

Let's have some fun with functions.
Implement a `fibonacci` function that returns a function (a closure) that returns successive [fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci_sequence) (0, 1, 1, 2, 3, 5, ...).
```go
func fibonacci() func() int {
	a, b := 0, 1
	
	return func() int {
		result := a
		a, b = b, a + b
		return result	
	}
}

func main(){
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
// 0
// 1
// 1
// 2
// 3
// 5
// 8
// 13
// 21
// 34
```

1. We initialize `a` and `b`:`a` and `b` start as `0` and `1`, which are the first two numbers in the Fibonacci sequence.
2. Closure Function:
	- The closure function captures `a` and `b` from the surrounding scope.
	- Each time the closure is called:
		- It stores the current value of `a` in `result`.
		- It then updates `a` to be `b` and `b` to be `a + b`, effectively moving to the next numbers in the Fibonacci sequence.
		- Finally, it returns the `result`, which is the current Fibonacci number.
3. Calling the Closure:
	- In `main`, we call `f()` in a loop to print the first 10 Fibonacci numbers.
