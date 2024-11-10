There can be many different binary trees with the same sequence of values stored in it. For example, here are two binary trees storing the sequence 1, 1, 2, 3, 5, 8, 13
![[Pasted image 20241109162803.png]]

A function to check whether two binary trees store the same sequence is quite complex in most languages. Let's use Go's concurrency and channels to write a simple solution.
We'll also use the `tree` package, which defines the type:
```go
type Tree struct {
	Left *Tree
	Value int
	Right *Tree
}
```

**1.** Implement the `Walk` function.
**2.** Test the `Walk` function.
The function `tree.New(k)` constructs a randomly-structured (but always sorted) binary tree holding the values `k`, `2k`, `3k`, ..., `10k`.
Create a new channel `ch` and kick off the walker:
go Walk(tree.New(1), ch)
Then read and print 10 values from the channel. It should be the numbers 1, 2, 3, ..., 10.
**3.** Implement the `Same` function using `Walk` to determine whether `t1` and `t2` store the same values.
**4.** Test the `Same` function.
`Same(tree.New(1), tree.New(1))` should return true, and `Same(tree.New(1), tree.New(2))` should return false.

The doc for `Tree` can be found [here](https://pkg.go.dev/golang.org/x/tour/tree#Tree).

```go
package main

import (
	"fmt"
	"golang.org/x/tour/tree"
)

// Walk recursively walks the binary tree `t` and sends its values to the channel `ch`.
func Walk(t *tree.Tree, ch chan int) {
	// Define a helper function for in-order traversal.
	var walkInOrder func(*tree.Tree)
	walkInOrder = func(t *tree.Tree) {
		if t == nil {
			return
		}
		// Visit left subtree, send value, visit right subtree
		walkInOrder(t.Left)
		ch <- t.Value
		walkInOrder(t.Right)
	}

	walkInOrder(t) // Start the traversal
	close(ch)      // Close channel once done
}

// Same checks if two trees contain the same values in the same order.
func Same(t1, t2 *tree.Tree) bool {
	ch1, ch2 := make(chan int), make(chan int)

	// Start concurrent walkers for each tree.
	go Walk(t1, ch1)
	go Walk(t2, ch2)

	// Compare values from both channels
	for v1 := range ch1 {
		v2, ok := <-ch2
		if !ok || v1 != v2 {
			return false
		}
	}

	// Check if ch2 is exhausted (both trees fully traversed)
	_, ok := <-ch2
	return !ok
}

func main() {
	// Test Walk function
	ch := make(chan int)
	go Walk(tree.New(1), ch)
	for v := range ch {
		fmt.Println(v)
	}

	// Test Same function
	fmt.Println("Same tree.New(1), tree.New(1):", Same(tree.New(1), tree.New(1))) // Should return true
	fmt.Println("Same tree.New(1), tree.New(2):", Same(tree.New(1), tree.New(2))) // Should return false
}
```

1. `Walk` function:
	- The function `Walk` uses an inner function `WalkInOrder` to recursively perform an in-order traversal on the tree.
	- Each node's value is sent to the channel `ch`.
	- Once the traversal is done, `close(ch)` is called to signal that no more values will be sent.
2. `Same` function:
	- This function uses two channels to collect values from `Walk` for each tree (`t1` and `t2`).
	- It reads values from both channels and compares them one by one.
	- If any mismatch occurs, it returns `false`.
	- After reading all values from `ch1`, it checks if `ch2` is also fully exhausted to ensure both trees had the same number of elements.
3. Output and Expected Results:
	- The `Walk` function test prints values from `tree.New(1)`, expected to be in sorted order from 1 to 10.
	- The `Same` function test will return `true` for identical trees and `false` for tress generated with different root values (like `tree.New(1)` and `tree.New(2)`).