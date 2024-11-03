Implement `Pic`. It should return a slice of length `dy`, each element of which is a slice of `dx` 8-bit unsigned integers. When you run the program, it will display your picture, interpreting the integers as grayscale (well, bluescale) values.
The choice of image is up to you. Interesting functions include `(x+y)/2`, `x*y`, and `x^y`.
(You need to use a loop to allocate each `[]uint8` inside the `[][]uint8`.)
(Use `uint8(intValue)` to convert between types.)

So basically, the function `Pic(dx, dy int) [][]uint8` should generate a 2D slice (array) of `uint8` values, representing pixel intensity values for an image. When this function is passed to `pic.Show`, it will render these values as an image, using shades of blue to represent different values.

```go
package main

import (
	"golang.org/x/tour/pic"
)

func Pic(dx, dy int) [][]uint8 {
	pixels := make([][]uint8, dy)

	for y := 0; y < dy; y++ {
		row := make([]uint8, dx)

		for x := 0; x < dx; x++ {
			row[x] = uint8(x * y) // <- you can change this logic with any of the interesting functions
		}

		pixels[y] = row
	}

	return pixels
}

func main(){
	pic.Show(Pic)
}
```