# wtfgolang

What the f\*ck Golang? 🤨

## Table of Contents

- [❓len("string") is Not Really the Length of a String](#lenstring-is-not-really-the-length-of-a-string)
- [❓range, Why Can’t You Be Like the Other Kids?](#range-why-cant-you-be-like-the-other-kids)
- [❓range, Why Can’t You Be Like the Other Kids? (Part 2)](#range-why-cant-you-be-like-the-other-kids-part-2)
- [❓When nil \!= nil](#when-nil--nil)
- [❓Slicing slices in Go might be a bit tricky](#slicing-slices-in-go-might-be-a-bit-tricky)
- [❓Named return values in Go might be a bit tricky](#named-return-values-in-go-might-be-a-bit-tricky)

### ❓`len("string")` is Not Really the Length of a String

```go
package main

import "fmt"

func main() {
  fmt.Println(len("Fіve"))
}
```

**Output**

```go
5
```

Well, it's obviously 5. Isn't it? 😅  
Don't trust me? Run it yourself in [The Go Playground](https://go.dev/play/p/BUU_QMh6xLl) 😜

#### 💡 Explanation

The `len("string")` function in Go returns the number of bytes, not the actual count of characters in a string. While this often coincides with the character count for ASCII strings (where each character is 1 byte), it can be misleading when dealing with Unicode characters, which may occupy more than one byte.

In this example character `і` (the Cyrillic letter “і”) is encoded in UTF-8 as two bytes, while the other characters F, v, and e are single-byte ASCII characters.

When dealing with multilingual strings or special characters, len might not give an accurate character count. To get the actual count of characters (or runes), you can use the `utf8.RuneCountInString` function from the `unicode/utf8` package:

```go
import (
  "fmt"
  "unicode/utf8"
)

func main() {
  fmt.Println(utf8.RuneCountInString("Fіve")) // Output: 4
}
```

In this case, `utf8.RuneCountInString("Fіve")` correctly returns 4, reflecting the true character count.

---

### ❓`range`, Why Can’t You Be Like the Other Kids?

```go
package main

import "fmt"

func main() {
  s := "Hі!"
  for char := range s {
    fmt.Println(char)
  }
}
```

**Output**

```go
0
1
3
```

Like, why indexes instead of characters? Why would you skip 2? 😅 Why can’t you just be like the other kids?

Example available in [The Go Playground](https://go.dev/play/p/TlHnZc84Yqp)

#### 💡 Explanation

Just keep in mind that the output is not the characters of the string. It's the byte index of the string.

---

### ❓`range`, Why Can’t You Be Like the Other Kids? (Part 2)

```go
package main

import "fmt"

func main() {
  s := "Hі!"
  for _, char := range s {
    fmt.Println(char)
  }
}
```

**Output**

```go
72
1110
33
```

No, seriously, why are you like this? 😂

Example available in [The Go Playground](https://go.dev/play/p/c_eAwsGYJRE)

#### 💡 Explanation

Anyone? Help?

---

### ❓When `nil != nil`

```go
package main

import "fmt"

type MyError struct{}

func (c MyError) Error() string {
  return "I ❤️ Golang"
}

func returnMyError() error {
  var err *MyError = nil
  return err
}

func main() {
  err := returnMyError()
  fmt.Println(err) // this prints <nil>
  if err == nil {
    fmt.Println("Of course it's nil, right?")
  } else {
    fmt.Println("Well, actually it's not 😄")
  }
}
```

**Output**

```go
<nil>
Well, actually it's not 😄
```

Example available in [The Go Playground](https://go.dev/play/p/sg78qnOtqpe)

#### 💡 Explanation

In `returnMyError`, we set `err` to `nil` as a `MyError` pointer and return it as an error interface. Here’s the catch: when we do this, the error interface keeps track of both the `MyError` type and the `nil` value. So, even though the value is `nil`, the interface itself isn’t fully empty — it still knows it should hold a `MyError` type. That’s why, when we check `if err == nil` in main, Go says it’s not `nil` because the interface has type information. In short, `nil != nil` here because the interface has a type, even though the value is nil.

```go
package main

import (
  "fmt"
  "reflect"
)

type MyError struct{}

func (c MyError) Error() string {
  return "I ❤️ Golang"
}

func returnMyError() error {
  var err *MyError = nil
  return err
}

func main() {
  err := returnMyError()
  fmt.Printf("Value of err: %v\n", err) // Prints <nil>
  fmt.Printf("Type of err: %T\n", err) // Shows that err has type *MyError

  if err == nil {
    fmt.Println("err is nil")
  } else {
    fmt.Println("err is NOT nil")
  }
}
```

**Output**

```go
Value of err: <nil>
Type of err: *main.MyError
err is NOT nil
```

---

### ❓Slicing slices in Go might be a bit tricky

```go
package main

import "fmt"

func main() {
 a := []int{1, 2, 3}
 b := a[:1]

 fmt.Println(a) // [1 2 3]
 fmt.Println(b) // [1]

 c := b[:3]
 fmt.Println(c) // ???
}
```

**Output**

```go
[1 2 3]
[1]
[1 2 3]
```

No comments 😅

Example available in [The Go Playground](https://go.dev/play/p/96HhnPJ_77K)

#### 💡 Explanation

Anyone? Help?

---

### ❓Named return values in Go might be a bit tricky

```go
package main

import (
	"errors"
	"fmt"
)

func doSomeWork() (string, error) {
	return "", errors.New("some error")
}

func doSomeCleaning() error {
	return nil
}

func run() (err error) {
	_, err = doSomeWork()
	fmt.Printf("error during calling doSomeWork() func: %+v\n", err)

	defer func() {

		if err = doSomeCleaning(); err != nil {
			fmt.Println("something went wrong")
		}
		fmt.Printf("defer successful witout error. Error: %+v\n", err)

	}()
	return err
}

func main() {
	fmt.Printf("Return from run() func: %+v", run())

}
```

**Output**

```go
error during calling doSomeWork() func: some error
defer successful without error. Error: <nil>
Return from run() func: <nil>
```

No comments 😅

Example available in [The Go Playground](https://go.dev/play/p/Moe1xZyd6Y2)

btw, if you change the named return `err` to `var err error` in `run` function, the output will be different. Can you guess why?

```go
package main

import (
	"errors"
	"fmt"
)

func doSomeWork() (string, error) {
	return "", errors.New("some error")
}

func doSomeCleaning() error {
	return nil
}

func run() error {
	var err error
	_, err = doSomeWork()
	fmt.Printf("error during calling doSomeWork() func: %+v\n", err)

	defer func() {

		if err = doSomeCleaning(); err != nil {
			fmt.Println("something went wrong")
		}
		fmt.Printf("defer successful witout error. Error: %+v\n", err)

	}()
	return err
}

func main() {
	fmt.Printf("Return from run() func: %+v", run())

}
```

Output:

```go
error during calling doSomeWork() func: some error
defer successful witout error. Error: <nil>
Return from run() func: some error
```

Example available in [The Go Playground](https://go.dev/play/p/0M1f0lxc6rf)

#### 💡 Explanation

Anyone? Help?
