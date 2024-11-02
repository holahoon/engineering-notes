Slices can contain any type, including other slices.
```go
func main(){
	// Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// The players take turns.
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][2] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Println("%s\n", strings.Join(board[i], " "))
		// X _ X
		// O _ X
		// _ _ O
	}
}
```