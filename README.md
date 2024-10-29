# wtfgolang

What the f\*ck Golang? 🤨

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

The len("string") function in Go returns the number of bytes, not the actual count of characters in a string. While this often coincides with the character count for ASCII strings (where each character is 1 byte), it can be misleading when dealing with Unicode characters, which may occupy more than one byte.

In this example character і (the Cyrillic letter “і”) is encoded in UTF-8 as two bytes, while the other characters F, v, and e are single-byte ASCII characters.

When dealing with multilingual strings or special characters, len might not give an accurate character count. To get the actual count of characters (or runes), you can use the utf8.RuneCountInString function from the unicode/utf8 package:

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

Ok it's 2 AM and I'm going to bed now. I'll update this tomorrow. 😴

---
