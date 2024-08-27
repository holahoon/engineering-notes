## Pointers
[docs](https://gobyexample.com/pointers)

Let's make a `Wallet` struct which lets us deposit `Bitcoin`.

`wallet_test.go`
```go
func TestWallet(t *testing.T) {
	wallet := Wallet{}
	wallet.Deposit(10)
	
	got := wallet.Balance()
	want := 10

	if got != want {
		t.Errorf("got %d want %d", got, want)
	}
}
```

`wallet.go`
```go
func (w Wallet) Deposit(amount int) {
	w.balance += amount
}

func (w Wallet) Balance() int {
	return w.balance
}
```

The test should pass right?
Well, not quite.

In Go, **When you call a function or a method the arguments are *copied***.
When calling `func (w Wallet) Deposit(amount int)`, the `w` is a copy of whatever we called the method from.

Well, we can find out what the *address* of that bit of memory with `&myVal`.
`wallet_test.go`
```go
func TestWallet(t *testing.T) {
	wallet := Wallet{}
	wallet.Deposit(10)
	got := wallet.Balance()
	fmt.Printf("address of balance in test is %p \n", &wallet.balance)
	want := 10
	if got != want {
		t.Errorf("got %d want %d", got, want)
	}
}
```

`wallet.go`
```go
func (w Wallet) Deposit(amount int) {
	fmt.Printf("address of balance in Deposit is %p \n", &w.balance)
	w.balance += amount
}
```

Here, the `%p` placeholder prints the memory address in base 16 notation with leading `0x`s and escape character prints a new line.
If we run the test now, we should get something like:
```
address of balance in Deposit is 0x1400000e1b0
address of balance in test is 0x1400000e1a8
```

We can fix this with *pointers*. Pointers let us *point* to some values and then let us change them.

`wallet.go`
```go
func (w *Wallet) Deposit(amount int) {
	w.balance += amount
}

func (w *Wallet) Balance() int {
	return w.balance
}
```

The difference now is the receiver type is `*Wallet` rather than `Wallet` which you can read as "a pointer to a wallet".

## Errors

What if we write a `Withdraw` function to allow withdrawals. It's pretty much the same as `Deposit`.

`wallet_test.go`
```go
func TestWallet(t *testing.T) {
	assertBalance := func(t testing.TB, wallet Wallet, want Bitcoin) {
		t.Helper()
		got := wallet.Balance()
		if got != want {
			t.Errorf("got %s want %s", got, want)
		}
	}

	t.Run("deposit", func(t *testing.T) {
		wallet := Wallet{}
		wallet.Deposit(Bitcoin(10))
		assertBalance(t, wallet, Bitcoin(10))
	})

	t.Run("withdraw", func(t *testing.T) {
		wallet := Wallet{balance: Bitcoin(20)}
		wallet.Withdraw(Bitcoin(10))
		assertBalance(t, wallet, Bitcoin(10))
	})

}
```

`wallet.go`
```go
func (w *Wallet) Withdraw(amount Bitcoin) {
	w.balance -= amount
}
```

What would happen if you try to `Withdraw` more than the balance in the account?
How do we signal a problem when using `Withdraw`?
In Go, if you want to indicate an error, it is idiomatic for your function to return an `err` for the caller to check and act on.

Let's refactor the test code:
`wallet_test.go`
```go
...
t.Run("withdraw insufficient funds", func(t *testing.T) {
	startingBalance := Bitcoin(20)
	wallet := Wallet{startingBalance}
	err := wallet.Withdraw(Bitcoin(100)) // <- returns an `err`

	assertBalance(t, wallet, startingBalance)

	if err == nil {
		t.Error("wanted an error but didn't get one")
	}
})
...
```

We want `Withdraw` to return an error if you try to take out more than the balance, and the balance should remain.
In our test code, we check for an error has returned by failing the test if it is `nil`.

`nil` is synonymous with `null` from other programming languages. Errors can be `nil` because the return type of `Withdraw` will be `error`, which is an interface. If you see a function that takes arguments or returns values that are interfaces, they can be nillable.

Like `null`, if you try to access a value that is `nil`, it will throw a **runtime panic**, so make sure to check for nils.

We now need to refactor our `Withdraw` function to account for the overdraft case:
`wallet.go`
```go
import (
	"errors"
)

...
func (w *Wallet) Withdraw(amount Bitcoin) error {
	if w.balance < amount {
		return errors.New("oh no")
	}

	w.balance -= amount
	return nil
}
```

Remember to import `errors` into your code.
`errors.New` creates a new `error` with a message of your choice.

### Refactor

We can do a clean up by refactoring our code:

`wallet.go`
```go
var ErrInsufficientFunds = errors.New("cannot withdraw, insufficient funds")

func (w *Wallet) Withdraw(amount Bitcoin) error {
	if w.balance < amount {
		return ErrInsufficientFunds
	}

	w.balance -= amount
	return nil
}
```

The `var` keyword allows us to define values global to the package.
This is a positive change in itself because now our `Withdraw` function looks a lot more clear.
Also, it'd be really annoying for the test to fail if someone wanted to re-word the error and it's just too much detail for our test. We don't really care about the exact wording of the error message.
In Go, errors are values, so we can just store it into a variable and have a single source of truth for it.

Let's refactor our test code as well:
`wallet_test.go`
```go
func TestWallet(t *testing.T) {

	t.Run("deposit", func(t *testing.T) {
		wallet := Wallet{}
		wallet.Deposit(Bitcoin(10))
		
		assertBalance(t, wallet, Bitcoin(10))
	})

	t.Run("withdraw with funds", func(t *testing.T) {
		wallet := Wallet{Bitcoin(20)}
		wallet.Withdraw(Bitcoin(10))
		
		assertBalance(t, wallet, Bitcoin(10))
	})

	t.Run("withdraw insufficient funds", func(t *testing.T) {
		wallet := Wallet{Bitcoin(20)}
		err := wallet.Withdraw(Bitcoin(100))

		assertError(t, err, ErrInsufficientFunds)
		assertBalance(t, wallet, Bitcoin(20))
	})
}

func assertBalance(t testing.TB, wallet Wallet, want Bitcoin) {
	t.Helper()
	got := wallet.Balance()

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}

func assertError(t testing.TB, got, want error) {
	t.Helper()
	if got == nil {
		t.Fatal("didn't get an error but wanted one")
	}

	if got != want {
		t.Errorf("got %q, want %q", got, want)
	}
}
```

Here, we've introduced `t.Fatal` which will stop the test if it is called.
