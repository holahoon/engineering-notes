Let's write a program which counts down from 3, printing each number on new line (with a 1-second pause) and when it reaches zero, it will print "Go!" and exit.
```
3
2
1
Go!
```

Let's start with a test code:
`countdown_test.go`
```go
func TestCountdown(t *testing.T) {
	buffer := &bytes.Buffer{}

	Countdown(buffer)

	got := buffer.String()
	want := `3
2
1
Go!`

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

`countdown.go`
```go
import (
	"fmt"
	"io"
	"os"
	"time"
)
const finalWord = "Go!"
const countdownStart = 3

func Countdown(out io.Writer) {
	for i := countdownStart; i > 0; i-- {
		fmt.Fprintln(out, i)
		time.Sleep(1 * time.Second)
	}

	fmt.Fprint(out, finalWord)
}
```

Nice, the tests still pass and the software works as intended, but we have some problems:
- Out tests take 3 seconds to run.
	- Every forward-thinking post about software development emphasizes the importance of quick feedback loops.
	- **Slow tests ruin developer productivity**.
	- Image if the requirements get more sophisticated warranting more tests. We definitely are not happy with 3 seconds added to the test run for every new test of `Countdown`.
- We have not tested an important property of our function.

We have a dependency on `Sleep`ing which we need to extract so we can then control it in our tests.

If we can *mock* `time.Sleep` we can use *dependency injection* to use it instead of a "real" `time.Sleep` and then we can **spy on the calls** to make assertions on them.





