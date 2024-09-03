Declaring a Map is somewhat similar to an array. Except, it starts with the `map` keyword and requires two types. The first is the key type, which is written inside the `[]`. The second is the value type, which goes right after the `[]`.
```go
dictionary := map[string]string{"test": "this is just a test"}
```

The key type is special. It can only be a comparable type because without the ability to tell if 2 keys are equal, we have no way to ensure that we are getting the correct value.
The value type, on the other hand, can be any type you want.

Let's practice by writing a test:
`dictionary_test.go`
```go
func assertStrings(t testing.TB, got, want string) {
	t.Helper()
	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}

func TestSearch(t *testing.T) {
	dictionary := map[string]string{"test": "this is just a test"}

	got := Search(dictionary, "test")
	want := "this is just a test"

	assertStrings(t, got, want)
}
```

`dictionary.go`
```go
func Search(dictionary map[string]string, word string) string {
	return dictionary[word]
}
```

### Refactor 1.
We can improve our dictionary's usage by creating a new type around map and making `Search` a method.
`dictionary_test.go`
```go
func TestSearch(t *testing.T) {
	dictionary := Dictionary{"test": "this is just a test"}

	got := dictionary.Search("test")
	want := "this is just a test"

	assertStrings(t, got, want)
}
```
Notice that we use `Dictionary` type and called `Search` on the `Dictionary` instance.

`dictionary.go`
```go
type Dictionary map[string]string

func (d Dictionary) Search(word string) string {
	return d[word]
}
```
Here we created a `Dictionary` type which acts as a thin wrapper around `map`. With the custom type defined, we can create the `Search` method.

### Refactor 2.
What if we supply a word that's not in our dictionary? We don't get anything back. This is kind of a good news because the program can continue to run, but there's a better approach. Make the function that reports that the word is not in the dictionary.
`dictionary_test.go`
```go
func assertError(t testing.TB, got, want error) {
	t.Helper()

	if got != want {
		t.Errorf("got error %q want %q", got, want)
	}
}

func TestSearch(t *testing.T) {
	dictionary := Dictionary{"test": "this is just a test"}

	t.Run("known word", func(t *testing.T) {
		got, _ := dictionary.Search("test")
		want := "this is just a test"
		assertStrings(t, got, want)
	})

	t.Run("unknown word", func(t *testing.T) {
	_, got := dictionary.Search("unknown")
	if got == nil {
		t.Fatal("expected to get an error.")
	}
	assertError(t, got, ErrNotFound)
})
}
```

`dictionary.go`
```go
var ErrNotFound = errors.New("could not find the word")

func (d Dictionary) Search(word string) (string, error) {
	definition, ok := d[word]
	if !ok {
		return "", ErrNotFound
	}

	return definition, nil
}
```
Notice that we use an interesting property of the map lookup. It can return 2 values. The second value is a boolean which indicates if the key was found successfully.

## Write to the dictionary
`dictionary_test.go`
```go
func TestAdd(t *testing.T) {
	dictionary := Dictionary{}
	dictionary.Add("test", "this is just a test")

	want := "this is just a test"
	got, err := dictionary.Search("test")
	if err != nil {
		t.Fatal("should find added word:", err)
	}

	assertStrings(t, got, want)
}
```

`dictionary.go`
```go
func (d Dictionary) Add(word, definition string) {
	d[word] = definition
}
```
Adding to a map is also similar to an array. You just specify a key and set it equal to a value.

Let's refactor our code a little bit:
`dictionary_test.go`
```go
func assertDefinition(t testing.TB, dictionary Dictionary, word, definition string) {
	t.Helper()

	got, err := dictionary.Search(word)
	if err != nil {
		t.Fatal("should find added word:", err)
	}
	assertStrings(t, got, definition)
}

func TestAdd(t *testing.T) {
	t.Run("new word", func(t *testing.T) {
		dictionary := Dictionary{}
		word := "test"
		definition := "this is just a test"

		err := dictionary.Add(word, definition)

		assertError(t, err, nil)
		assertDefinition(t, dictionary, word, definition)
	})

	t.Run("existing word", func(t *testing.T) {
		word := "test"
		definition := "this is just a test"
		dictionary := Dictionary{word: definition}
		err := dictionary.Add(word, "new test")

		assertError(t, err, ErrWordExists)
		assertDefinition(t, dictionary, word, definition)
	})
}
```

`dictionary.go`
```go
var (
	ErrNotFound   = errors.New("could not find the word you were looking for")
	ErrWordExists = errors.New("cannot add word because it already exists")
)

func (d Dictionary) Add(word, definition string) error {
	_, err := d.Search(word)

	switch err {
	case ErrNotFound:
		d[word] = definition
	case nil:
		return ErrWordExists
	default:
		return err
	}

	return nil
}
```

### Pointers, copies, et al
An interesting property of maps is that you can modify them without passing as an address to it (e.g. `&myMap`).
This may make them *feel* like a "reference type", but they are not. A map value is a pointer to a *runtime.hmap* structure. [ref](https://dave.cheney.net/2017/04/30/if-a-map-isnt-a-reference-variable-what-is-it).
So when you pass a map to a function/method, you are indeed copying it, but just the pointer part, not the underlying data structure that contains the data.

A gotcha with maps is that they can be a `nil` value. A `nil` map behaves like an empty map when reading, but attempts to write to a `nil` map will cause a runtime panic.
Therefore, never initialize a nil map variable:
```go
var m map[string]string
```

Instead, initialize an empty map or use the `make` keyword to create a map for you:
```go
var dictionary = map[string]string{}
// OR
var dictionary = make(map[string]string)
```
Both approaches create an empty `hash map` and point `dictionary` at it. This ensures that you never get a runtime panic.

## Update to the dictionary
Let's add create a function to `Update`  the definition of a word.

`dictionary_test.go`
```go
func TestUpdate(t *testing.T) {
	t.Run("existing word", func(t *testing.T) {
		word := "test"
		definition := "this is just a test"
		dictionary := Dictionary{word: definition}
		newDefinition := "new definition"
	
		err := dictionary.Update(word, newDefinition)
	
		assertError(t, err, nil)
		assertDefinition(t, dictionary, word, newDefinition)
	})
	
	t.Run("new word", func(t *testing.T) {
		word := "test"
		definition := "this is just a test"
		dictionary := Dictionary{}
	
		err := dictionary.Update(word, definition)
	
		assertError(t, err, ErrWordDoesNotExist)
	})
}
```

`dictionary.go`
```go
const (
	ErrNotFound         = DictionaryErr("could not find the word you were looking for")
	ErrWordExists       = DictionaryErr("cannot add word because it already exists")
	ErrWordDoesNotExist = DictionaryErr("cannot update word because it does not exist")
)

type DictionaryErr string

func (e DictionaryErr) Error() string {
	return string(e)
}

func (d Dictionary) Update(word, definition string) error {
	_, err := d.Search(word)

	switch err {
	case ErrNotFound:
		return ErrWordDoesNotExist
	case nil:
		d[word] = definition
	default:
		return err
	}

	return nil
}
```
Notice that we made the errors constant; this required us to create our own `DictionaryErr` type which implements the `error` interface. This makes the errors more reusable and immutable. - [article](https://dave.cheney.net/2016/04/07/constant-errors).

## Delete to the dictionary
Let's create a function to `Delete` a word in the dictionary.
`dictionary_test.go`
```go
func TestDelete(t *testing.T) {
	word := "test"
	dictionary := Dictionary{word: "test definition"}

	dictionary.Delete(word)

	_, err := dictionary.Search(word)
	assertError(t, err, ErrNotFound)
}
```

`dictionary.go`
```go
func (d Dictionary) Delete(word string) {
	delete(d, word)
}
```

Go has a built-in function `delete` that works on maps. It takes two arguments. The first is the map and the second is the key to be removed.
The `delete` function returns nothing, and we based our `Delete` method on the same notion. Since deleting a value that's not there has no effect, unlike our `Update` or `Add` methods, we don't need to complicate the API with errors.