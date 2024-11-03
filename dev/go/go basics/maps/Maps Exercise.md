Implement `WordCount`. It should return a map of the counts of each “word” in the string `s`. The `wc.Test` function runs a test suite against the provided function and prints success or failure.

You might find [strings.Fields](https://go.dev/pkg/strings/#Fields) helpful.

```go
package main

import (
	"golang.org/x/tour/wc"
	"strings"
)

func WordCount(s string) map[string]int {
	wordCount := make(map[string]int)
	words := string.Fields(s)

	for _, word := range words {
		wordCount[word]++
	}

	return wordCount
}

func main(){
	wc.Test(WordCount)
}
```