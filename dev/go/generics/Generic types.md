In addition to generic functions, Go also supports generic types. A type can be parameterized with a type parameter, which could be useful for implementing generic data structures.

The below example demonstrates a simple type declaration for a singly-linked list holding any type of value.

```go
package main

type List[T any] struct {
	next *List[T]
	val T
}

func (l *List[T]) Append(val T) {
	current := l
	for current.next != nil {
		current = current.next
	}
	current.next = &List[T]{val: val}
}

func (l *List[T]) Prepend(val T) *List[T] {
	newHead := &List[T]{val: val, next: l}
	return newHead
}

func (l *List[T]) Print() {
	current := l
	for current != nil {
		fmt.Println("%v -> ", current.val)
		current = current.next
	}
	fmt.Println("nil")
}

func (l *List[T]) Length() int {
	count := 0
	current := l
	for current != nil {
		count++
		current = current.next
	}
	return count
}

func main() {
	list := &List[int]{val: 1}
	
	list.Append(2)
	list.Append(3)
	list.Append(4)
	
	fmt.Println("Initial list: ")
	list.Print()
	
	fmt.Printf("Length of the list: %d\n", list.Length())
	
	list = list.Prepend(0)
	fmt.Println("After prepending 0:")
	list.Print()
}
// Initial list: 
// %v ->  1
// %v ->  2
// %v ->  3
// %v ->  4
// nil
// Length of the list: 4
// After prepending 0:
// %v ->  0
// %v ->  1
// %v ->  2
// %v ->  3
// %v ->  4
// nil
```